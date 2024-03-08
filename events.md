apisix 的事件模块支持lua-resty-worker-events和lua-resty-events两个库，其中lua-resty-worker-events是默认的，lua-resty-events需要手动配置。

https://konghq.com/blog/engineering/nginx-openresty-event-handling-strategy-for-cpu-efficiency
kong团队使用unix socket来处理事件分发

本文介绍了Kong Gateway使用Nginx/OpenResty的CPU效率事件处理策略，包括其master/worker架构的优缺点以及解决信息沟通问题的方法。文章介绍了两种事件传播机制的原理和区别，以及为什么选择开发新的事件库。

要点提炼：

Kong Gateway使用Nginx/OpenResty的master/worker架构，每个worker是隔离进程，难以共享信息。
事件传播机制是解决信息沟通问题的方法，包括基于共享内存的lua-resty-worker-events和基于定时器的lua-resty-events。
lua-resty-worker-events是Kong Gateway的核心组件，依赖于它实现的重要功能包括健康检查、集群同步和更新路由/目标。
lua-resty-worker-events存在一些缺点，如访问共享内存需要使用锁，当有很多worker时锁开销很大；使用OpenResty API ngx.timer.at轮询事件。
Kong从3.0版本开始使用新的基于定时器的lua-resty-events库来替代lua-resty-worker-events。


