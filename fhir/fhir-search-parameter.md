## HAPI新增非标准搜索参数
FHIR标准中各资源的可选择搜索条件是有限的，如果需要增加额外的搜索条件，需要增加自定义搜索条件；
相关链接:
 - [FHIR searchparameter 是什么](https://www.hl7.org/fhir/searchparameter.html)
 - [Official searchparameter 标准搜索参数的定义](https://www.hl7.org/fhir/search-parameters.json)

>以下测试在HAPI-FHIR基础上展开

**实现步骤：**
1. 创建自定义的SearchParameter资源；
   *需注意status等字段，可能导致parameter不启用；*
   *可参考上面链接2中的标准定义；*
1. 触发Re-index，或者新建符合条件的数据；
   *HAPI re-index task默认启动，配置项‘hapi.fhir.jpa.scheduling-disabled’（默认false）若为true可能导致任务不启动*
