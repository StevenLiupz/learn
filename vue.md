## 计算属性 - computed
计算属性是用来声明式的描述一个值依赖了其他的值。当你在模板里把数据绑定到一个计算属性上时，Vue会在其依赖的任何值导致该计算属性改变时更新DOM。  
computed相当于method，返回function内return的值渲染到DOM上。  
######相比于method，即使多个{{}}里面使用了computed计算属性，conputed内的function也只执行一次，仅当function内涉及到Vue实例绑定的data的值的改变，function才会重新执行。而如果多个{{}}里面调用了method里面的方法，就会执行多次。
### 观察 watch
当想要在数据变化响应时，执行异步操作或开销较大的操作，可以使用watch进行监听。