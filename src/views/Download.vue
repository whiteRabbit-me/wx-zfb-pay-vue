<template>
  <div class="bg-fa of">
    <section id="index" class="container">
      <header class="comm-title">
        <h2 class="fl tac">
          <span class="c-333">微信账单申请</span>
        </h2>
      </header>
      
      <el-form :inline="true" >
        <el-form-item>
            <el-date-picker v-model="billDate" value-format="yyyy-MM-dd" placeholder="选择账单日期" />
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="downloadBill('tradebill')">下载交易账单</el-button>
        </el-form-item>
         <el-form-item>
            <el-button type="primary" @click="downloadBill('fundflowbill')">下载资金账单</el-button>
        </el-form-item>
      </el-form>
    </section>

    <section id="index" class="container">
      <header class="comm-title">
        <h2 class="fl tac">
          <span class="c-333">支付宝账单申请</span>
        </h2>
      </header>
      
      <el-form :inline="true" >
        <el-form-item>
            <el-date-picker v-model="billDate_alipay" value-format="yyyy-MM-dd" placeholder="选择账单日期" />
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="downloadBillAliPay('trade')">下载交易账单</el-button>
        </el-form-item>
         <el-form-item>
            <el-button type="primary" @click="downloadBillAliPay('signcustomer')">下载资金账单</el-button>
        </el-form-item>
      </el-form>
    </section>

  </div>
</template>

<script>
import billApi from '../api/bill'

export default {
  data () {
    return {
       billDate: '', //微信支付账单日期
       billDate_alipay: '' //支付宝账单日期
    }
  },

  methods: {

    //下载账单：微信支付
    downloadBill(type){
      //获取账单内容
      billApi.downloadBillWxPay(this.billDate, type).then(response => {
        console.log(response)
        const element = document.createElement('a')
        element.setAttribute('href', 'data:application/vnd.ms-excel;charset=utf-8,' + encodeURIComponent(response.data.result))
        element.setAttribute('download', this.billDate + '-' + type)
        element.style.display = 'none'
        element.click()
      })
    },

    //下载账单：支付宝
    downloadBillAliPay(type){
        billApi.downloadBillAliPay(this.billDate_alipay, type).then(response => {
          console.log(response.data.downloadUrl)
          const element = document.createElement('a')
          element.setAttribute('href', response.data.downloadUrl)
          element.setAttribute('download', this.billDate_alipay + '-' + type)
          element.style.display = 'none'
          element.click()
        })
    }
  }
}
</script>