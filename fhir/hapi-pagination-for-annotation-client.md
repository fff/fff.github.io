## HAPI 注释方式客户端如何实现分页查询

FHIR标准没有RESTFul的翻页查询方式（指定size/page获取指定页码指定数量元素），但是HAPI扩展出了'_getpagesoffset'参数，结合FHIR标准里的'_count'参数，可以实现上诉期望的查询方式；

HAPI annoation client里没有翻页查询例子，下文是探索后实现的方式；

*遗留问题：annotation方式只能获取到页码内元素集合，无法得到关于资源总体信息的描述，total需要额外查询得到；*

相关链接：
 - [HAPI annotation client](http://hapifhir.io/doc_rest_client_annotation.html)
 - [@Count parameter](http://hapifhir.io/apidocs-dstu3/org/hl7/fhir/dstu3/model/Count.html)

List查询示例代码：
```java
  @Search
    List<CarePlan> find(
            @OptionalParam(name = CarePlan.SP_PATIENT) ReferenceParam patient,
            @OptionalParam(name = CarePlan.SP_DATE) DateRangeParam period,
            //OFFSET = pageIdx*pageSize
            @RequiredParam(name = Constants.PARAM_PAGINGOFFSET) NumberParam pagingOffset,
            //COUNT = pageSize
            @Count Integer pageSize
    );
```
Pagination的另一要素total，可通过fluent client方式获取，设置‘count=0’:
```java
iQuery.count(0).returnBundle(Bundle.class).execute().getTotal();
```

Summary:
- Pros: 需要针对不同client类型，维护两套查询条件setup逻辑；
- Cons: Annotation client大量节省了result parsing的工作量；