# flex布局

## 1、容器上的属性

```
flex-direction：主轴排序方式，可选值:row、row-reverse、column、column-reverse

flex-wrap：在主轴上换不换行，可选值：nowrap（默认）、wrap、wrap-reverse

flex-flow：是flex-direction与flex-wrap的简写方式，默认值为row nowrap

justify-content：在主轴上的对方式，
可选值：flex-start、flex-end、center、space-between、space-around

align-items：在交叉轴上如何对齐，可选值：flex-start、flex-end、center、baseline、stretch

align-content：多跟轴线对其方式，
可选值：flex-start、flex-end、center、space-between、space-around、stretch
```



## 2、元素上的属性

```
order：排序顺序，值越小越靠前

flex-grow：方大比例，默认0

flex-shrink：缩小比例，默认1

flex-basis：分配主轴多余空间，默认auto

flex：flex-grow + flex-shrink + flex-basis简写，默认值为0 1 auto

align-self：允许单个项目有与其他项目不一样的对齐方式，
可选值：auto、flex-start、flex-end、center、baseline、stretch

```

