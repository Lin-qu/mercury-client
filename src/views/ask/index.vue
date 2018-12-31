<template>
  <div>
    <Card title="提问">
      <Form label-position="top" :model="form">
        <FormItem prop="title" label="标题">
          <Input type="text" v-model="form.title" placeholder="请输入标题" />
        </FormItem>
        <FormItem prop="category" label="类别">
          <Select v-model="form.category" placeholder="请选择类别" v-for="category in category_list">
            <Option :value="category.id">{{ category.title }}</Option>
          </Select>
        </FormItem>
        <FormItem prop="content" label="内容">
          <Input type="textarea" v-model="form.content" placeholder="请输入内容" />
        </FormItem>
        <Button type="primary" @click="submit">提交</Button>
      </Form>
    </Card>
  </div>
</template>

<script>
export default {
  name: "Ask",
  data() {
    return {
      form: {
        title: "",
        category: 1,
        content: "",
      }
    };
  },
  data: {
    category_list:[
      {id:1, title:"技术"},
      {id:2, title:"生活"}
    ]
  },
  methods: {
    async submit() {
      let res = await this.$http.post("/api/ask/submit", this.form);
      console.log(res);
      if (res.status != 200) {
        this.$Message.error("提问失败，网络错误");
        return;
      }
      if (res.data.code === 0) {
        this.$Message.success("提问成功");
        this.$router.push("/");
      } else {
        this.$Message.error(res.data.message);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
</style>
