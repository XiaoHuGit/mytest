send_sms.php  <==>  发送短信验证码   <==>  request: post
	
	receive: 
			string  phone  <==> 手机号码
	return:
			success  <==> { errno: 0, errmsg: "" }
			failed   <==> { errno: 4031, errmsg: 短信发送失败 }
	
register.php  <==>  注册第一步  <==> request: post

	receive:
			string mobile  <==> 手机号码
			number note    <==> 短信验证码
	return:
			success  <==> { errno: 0, errmsg: "" }
			failed   <==> { errno: 4031, errmsg: 验证码错误 },
			              { errno: 4041, errmsg: 用户已注册 },
			              { errno: 500, errmsg: 服务器异常 }
			              
set_password.php  <==>  注册第二步设置密码 <==> request: post

	receive:
			string mobileno   <==> 用户id
			string password <==> 密码
	return:
			success  <==> { errno: 0, errmsg: "", data:{userid} } 
			failed   <==> { errno: 4031, errmsg: 设置密码失败 }
			
login.php  <==>  登录 <==> request: post

	receive:
			string  mobile   <==> 手机号码
			string  password <==> 密码      【可选, 普通登录必需】
			number  note     <==> 短信验证码 【可选, 快速登录必需】
			string  token    <==> 设备标识符  
	return:
			success  <==> { errno: 0, errmsg: "", data: ['imei':xx, 'userinfo': xx, 'encryptinfo': xx, 'sessionid': xx, 'token': xx] }(如果有token,即此设备为要被踢下)
			failed   <==> { errno: 4031, errmsg: 验证码错误 },
			              { errno: 404, errmsg: 用户不存在 },
			              { errno: 4032, errmsg: 密码错误 }
			            
validate_user.php <==> 修改密码第一步, 验证用户信息 <==> request: get

	receive:
			string  mobile <==> 手机号码
			number  note   <==> 短信验证码
	return:
	   		success  <==> { errno: 0, errmsg: "", data: ['userid': xx] }
	   		failed   <==> { errno: 4031, errmsg: 验证码错误 },
	   		              { errno: 404, errmsg: 用户不存在 },
	   		              
logout.php  <==> 退出登录 <==> request: post
	
	receive:
			int    userid  <==> 用户id
	return:
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 用户不存在 },
			             { errno: 500, errmsg: 服务器错误 }
trace.php  <==> 单设备追踪 <==> request: get

	receive:
			string imei   <==> 设备imei号
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
			success <==> { errno: 0, errmsg: "", data:[{'imei':设备号, 'status':设备状态, 'temper':温度, 'gps': GPS数值, 'gsm': GSM数值, lat':纬度, 'lon':经度, 'sensorid':设备id, fence':围栏数据}]
			failed  <==> { errno: 404, errmsg: 不合法的设备号 }
			             { errno: 4031, errmsg: 用户未还未绑定设备 }
			             { errno: 40311, errmsg: 无效的用户 }
trace_my.php <==> 多设备追踪 <==> request: get

	receive:
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
	       success <==> { errno: 0, errmsg: "", data:[{'imei':设备号, 'status':设备状态, 'temper':温度, 'gps': GPS数值, 'gsm': GSM数值, lat':纬度, 'lon':经度, 'sensorid':设备id, fence':围栏数据},{....},...]
	       failed  <==> { errno: 4031,  errmsg: 用户还未绑定设备 }
	                    { errno: 40311, errmsg: 无效的用户 }
path_back.php <==> 路径回放 <==> request: get

	receive:
			string imei   <==> 设备imei号
			date   time   <==> 指定查看日期
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
			success <==> { errno: 0, errmsg: "", sites: xx }
			failed  <==> { errno: 404, errmsg: 设备不存在 }
			             { errno: 40311, errmsg: 无效的用户 }
bind_sensor.php <==> 绑定设备 <==> request: post

	receive:
			int    userid <==> 用户id
			string imei   <==> 设备imei号
			string nick   <==> 设备昵称
			string token  <==> 请求令牌
	return: 
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 设备不存在 }
			             { errno: 403, errmsg: 绑定失败 }
			             { errno: 500, errmsg: 服务器错误 }
			             { errno: 40311, errmsg: 无效的用户 }
unbind_sensor.php <==> 解绑设备 <==> request: post
	
	receive:
			int    userid <==> 用户id
			array  imeis  <==> 设备imei号集合
			string token  <==> 请求令牌
	return:
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 设备不存在 }
			             { errno: 403, errmsg: 解绑失败 }
			             { errno: 500, errmsg: 服务器错误 }
			             { errno: 40311, errmsg: 无效的用户 }
journey_statistics.php <==> 路程统计 <==> request: get

	receive:
			string imei   <==> 设备imei号
			date   time   <==> 指定查看日期
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
			success <==> { errno: 0, errmsg: "", data: xx }
			failed  <==> { errno: 404, errmsg: 设备不存在 }
			             { errno: 40311, errmsg: 无效的用户 }
electric_fence.php <==> 设置电子围栏 <==> request: post
	
	receive:
			string imei   <==> 设备imei号
			int    radius <==> 半径
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 设备不存在 }
			             { errno: 4031, errmsg: 没有数据 }
			             { errno: 500, errmsg: 服务器错误 }
			             { errno: 40311, errmsg: 无效的用户 }
discard\_electric_fence.php <==> 取消电子围栏 <==> request: post

	receive: 
			string imei    <==> 设备imei号
			int    userid  <==> 用户id
			string token   <==> 请求令牌
	return:
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 设备不存在 }
			             { errno: 403, errmsg: 取消失败 }
			             { errno: 40311, errmsg: 无效的用户 }
upgrade_app <==> 检测是否要升级应用 <==> request: get
	
	receive: 
			none
	return:
			succes <==> { errno: 0, errmsg: "", data:[ 'version': xxx..]
kick.php <==> 检测是否需要被踢下线 <==> request: get
	
	receive: 
			int 	 uid	<==> 用户id(未登录时不需要传)
			string token	<==> 设备token
	return:
			success <==> { errno: 0, data:{ status: Y(踢下线) N(不踢）}
			failed  <==> { errno: 4030, errmsg: 未登录 }
change_nick.php <==> 更改设备昵称  <==> request: post

	receive:
			int    id     <==> 用户设备绑定的主键
			string nick   <==> 设备昵称
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
	      success <==> { errno: 0, errmsg: "", data:[] }
	      failed  <==> { errno: 4031, errmsg: 修改昵称失败 }
	                   { errno: 40311, errmsg: 无效的用户 }
show_sensor.php <==> 显示设备列表 <==> request: get

	receive:
			int    userid <==> 用户id
			string token  <==> 请求令牌
	return:
	       success <==> { errno: 0, errmsg: "", data:[{id:主键,nick:昵称,imei:设备号},{...},...]
	       failed  <==> { errno: 40311, errmsg: 无效的用户 }
enable_alarm.php <==> 启用报警设置 <==> request: post
	
	receive:
			int uid  <==> 用户id
			int type <==> 报警类型 [按报警取值:1:围栏,2:断电,4:拔出,8:震动]
	return:
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 用户不存在 } (开发模式)
			             { errno: 403, errmsg: 设置报警失败 }
disable_alarm.php <==> 关闭报警 <==> request: post

	receive:
			int uid  <==> 用户id
			int type <==> 报警类型 [同上]
	return:
			success <==> { errno: 0, errmsg: "" }
			failed  <==> { errno: 404, errmsg: 用户不存在 } (开发模式)
			             { errno: 403, errmsg: 关闭报警失败 }
show\_alarm_log.php <==> 显示报警列表 <==> request: get

	receive:
			int uid  <==> 用户id
			int page <==> 页码
	reture:
			success <==> { errno: 0, errmsg: "", data:[{id:主键,content:报警内容,imei:设备号},{...},...] }
remove\_alarm_log.php <==> 删除某个报警记录 <==> request: post

	receive:
			int id <==> 记录主键
	return:
	      success <==> { errno: 0, errmsg: "", data:[] }
	      failed  <==> { errno: 4031, errmsg: 删除记录失败 }
sensor\_status_statistics.php <==> 首页设备状态统计 <==> request: get

	receive:
			int uid <==> 用户id
	return:
			success <==> { errno: 0, errmsg: "", data:['all':全部车辆, 'online':在线,'off':断开,'malf':故障 }
show\_alarm_setting.php <==> 显示报警设置信息 <==> request: get
	
	receive:
			int uid <==> 用户id
	return:
			success <==> { errno: 0, errmsg: "", data: ['fence':0|1, 'off':0|1, 'out':0|1, 'shock':0|1 } (0代表关闭|1代表启动)
			
show\_sensors\_from_status.php <==> 根据状态显示设备列表信息 method: get

	receive:
			int		uid <==> 用户id 例: 896766
			string	status <==> 状态 例: "all"|"online"|"off"|"malf"
	return:
			success: {
				errno: 0,
				errmsg: "",
				data: [
					{
						id: 903196,
						sensorid: 892136,
						nick: 3333
						imei: 333333333333333
					},
					...
				]|[]
			}
			
forget_password.php <==> 忘记密码的重置密码 method: post

	receive:
			int		userid <==> 用户id 例: 896776
			string	password <==> 密码 例: dafjruewriejksjfk
	return: 
			success: {
				errno: 0,
				errmsg: "",
				data: []
			}
			failed: {
				errno: 403, errmsg: 修改密码失败
			}
			