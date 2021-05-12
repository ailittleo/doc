### 导出 excel 使用公共方法

```js
//  导出按钮触发的方法
    async exportExcel() {
      //给按钮添加loading样式
      this.excelLoading = true;
      //  请求接口
      const list = await exportTotalByQuoter(this.searchForm);
      import('@/vendor/Export2Excel').then(excel => {
        //需要导出的key对应的表头的name  根据项目需要进行一一对应
        const obj = {
          quoterName: '报价员',
          statusCount: '工单状态总数',
          quoterTeamName: '归属团队',
          quoterCityName: '归属地市',
          statusCalculationing: '报价中',
          statusCalculationSuccess: '报价成功',
          statusCalculationFail: '报价失败',
          statusUnderwritinging: '核保中',
          statusPayWait: '待支付',
          statusUnderwritingFail: '核保失败',
          statusUnderwritingLabour: '转人工核保',
          statusProcessed: '已出单'
        };
        const tHeader = [];
        const filterVal = [];
        Object.keys(obj).forEach(key => {
          filterVal.push(key);
          tHeader.push(obj[key]);
        });
        //格式化需要转换的值 比如字典值
        const data = this.formatJson(filterVal, list.data.data);
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: 'excel名称'
        });
        this.excelLoading = false;
      });
    },
    formatJson(filterVal, tableData) {
      return tableData.map(v => {
        return filterVal.map(j => {
          if (j === 'status') {
            return formateExcel(v[j], this.dicts.status);
          } else if (j === 'autoInsuredType') {
            return formateExcel(v[j], this.autoInsuredType);
          }else {
            return v[j];
          }
        });
      }
      );
    },

    // 后续补充多级表头导出方法
```
