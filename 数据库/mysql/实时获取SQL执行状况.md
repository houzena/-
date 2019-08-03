实时获取SQL执行状况

select id,user,host,db,command,time,state,info from information_schema.processlist;