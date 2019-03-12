React builds and maintains an internal representation of the rendered UI.

> React构建并维护“代表着已渲染的UI界面”的内部数据。

It includes the React elements you return from your components.

> 这些内部数据包括了从组件返回的所有React元素。

This representation lets React avoid creating DOM nodes and accessing existing ones beyond necessity, as that can be slower than operations on JavaScript objects.

> 有了代表着UI界面的内部数据，React就可以在多数情况下避免去创建和访问DOM节点了，除非是到了十分必要的时候。因为操作“DOM对象”比操作“Javascript对象”要慢一些。

Sometimes it is referred to as a “virtual DOM”, but it works the same way on React Native.

> 这种内部数据有时候被称为“虚拟DOM节点”。但其实在React Native上它也以同样的方式工作着。

When a component’s props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM.

> 当一个组件的属性或状态改变时，React会比较组件新返回的元素和已经被渲染出来的元素是否一致，并以此决定是否需要更新DOM节点。只有当新返回的元素和已经被渲染的元素不一致时，React才会进行DOM更新。

Even though React only updates the changed DOM nodes, re-rendering still takes some time.

> 即便React只更新有改变的那些DOM节点，重新渲染依旧比较耗时。

in many cases it’s not a problem, but if the slowdown is noticeable, you can speed all of this up by overriding the lifecycle function 

shouldComponentUpdate, which is triggered before the re-rendering process starts.

> 在大多数情况下，这不是一个问题。但如果你注意到程序慢下来了，就可以通过重写shouldComponentUpdate这个生命周期函数来进行加速。shouldComponentUpdate会在每次重新渲染前被执行。



Rendered UI	已渲染的用户界面

internal representation	内部表现

maintain  /menˈten/	维护

beyond /bɪˈjɑ:nd/	超过，超越

component /kəmˈpoʊnənt/	组件

avoid /əˈvɔɪd/	避免；避开

avoid doing sth.	避免做某事

existing /ɪɡˈzɪstɪŋ/	现存的；目前的

necessity  /nəˈsɛsɪti/	需要，必需品

access /ˈæksɛs/	进入、访问

refer to	引用

reference /ˈrɛfrəns/	引用；变量中的引用类型

Reference Error 	引用错误

virtual  /ˈvɜ:rtʃuəl/	虚拟的

Virtual Reality	虚拟现实

prop，全称为property，句子里用的为复数properties，属性

state /stet/	状态

newly /ˈnu:li/	新地

previously /ˈprivɪəslɪ/	先前地

compare  /kəmˈper/	比较

element /ˈɛləmənt/	元素

even though	即使，即便

changed /tʃendʒd/	改变的

take time	花费时间、耗时

in many cases	在很多情况下

noticeable  /ˈnoʊtɪsəbl/	明显的

speed sth. up	给…加速

slowdown  /ˈsloʊdaʊn/	减缓

override  /ˌoʊvərˈraɪd/	覆盖，重写

lifecycle function	生命周期函数

trigger /ˈtrɪɡɚ/	触发