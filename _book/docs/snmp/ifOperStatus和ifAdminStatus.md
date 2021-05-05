配置状态就是你配置这个端口是什么状态，比如是“up” or “down”。

例如：你配置端口的状态是“up”，ifAdminStatus是“up”。

但是如果这个端口没有插线，那么ifOperStatus是“down”。

备注：

链路状态是up–>管理状态一定是up。

管理状态是down–>链路状态一定是down。



ifAdminStatus的值为up(1) ，而ifOperStatus的值down(2) ，则该接口处于故障状态。如果对象if_AdminStatus、ifOperStatus的值均为up(1) ，则接口处于正常状态；如果对象ifAdminStatus、ifOperStatus的值均为down(2) ，则接口处于停机状态；如果对象ifAdminStatus、ifOperStatus的值均为Testing(3) ，则接口处于测试状态