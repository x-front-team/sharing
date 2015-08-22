##AngularJS曾经踩过的坑

1. form中的model，如果初始值`/$viewValue`不通过校验，其值将为null———好大的坑！
2. 指令的link函数，如果相应的模板里有指令，并且link函数里有ajax获取模板，并且获取到模板后修改DOM，会发生指令执行两次的情况（应该视该模板的返回先后而定，如果晚了就会出现这种情况）—————写form组件的坑!
3. 无论在同个模块或多个模块：
  1）同名指令将不会发生替换，按照依赖声明顺序，定义顺序的倒序执行
  2）同名service，同名filter,同名controller 将发生替换，后面的替换前面的  
4. select的请选择选项，不要放在dataList里，直接写在select里面，若要由选择性的输出该选项可以用ng-if
5. ng-show和ng-hide并不如想象中好用，它的本质只是控制ng-hide这个class的出现与否，详见angular源码
6. ng-transclude的默认scope是其指令所在的scope（如果在指令模板里用ng-transclude的话），如果想改变其scope，可以用compile或link里的transcludeFn，传入指定scope，并将其编译出来的片段插入到指定位置。
7. 父子指令，scope中的方法同名，可能不会触发
8. 在首次执行directive的时候，directiveFactory才会执行
9. scope should be write-only in the controller, and read-only in directives. 指令里的transclude，如果有属性从父scope传进来，最好不要直接修改它（比如传进来的是resources:’=‘,不要scope.resources = []这样改），否则需要使用多一层的对象，修改这个对象下的属性（即someData:’=‘,scope.someData.resources = [0]）补充：子视图中的ng-model也会有这样的问题！！！
10. 同一个元素上的多个指令，不能都设置新的scope
11. 在指令中，在angular之外的回调需要修改scope中的值，需要$apply回调，比如xhr的fileupload
12. var defer = $q.defer(); defer.resolve();          —————另一个地方：defer.promise.then(function(){//成功了})
13. var allPromise = $q.all([promise1,promise2,promise3…]) -----另一个地方:allPromise.then(function(){//成功了})
14. $viewValue是显示值，$modelValue是model值，比如$viewValue是字符串”300”，$modelValue是数字300，又如$viewValue是字符串”2014-12-17”，$modelValue是new Date(”2014-12-17”) javascript 对象，$setViewValue()内部会调用$parser 完成$viewValue到$modelValue,只不过大部分情况他们俩都是字符串，都是相等的
15. $scope.$evel是调用$parse服务的
16. angular-route 配置中的resolve函数中，$routeParams还未设置，应使用$route.current.params
17. 在服务里动态添加指令到body里时，比如对话框类的。。。往指令标签里写属性的时候，记得驼峰与连字符的转换。。。。
18. 必须得在指令的$watch回调中，指令父scope的绑定变量才会同步数据