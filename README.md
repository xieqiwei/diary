#2015-09-06

腾讯离职

markdown学习

##收费数据

###总体流量OD
- 计算OD总体流量（num,insation,inroad,station,road）
>
	SELECT
		count(*),
		201407_1.InStationNo,
		201407_1.InRoadNo,
	  201407_1.StationNo,
	  201407_1.RoadNo
	FROM
		201407_1
	WHERE
		OpTime LIKE "2014-07%" 
	AND CAST(OpTime AS datetime) BETWEEN '2014-07-01' AND '2014-07-21'
	AND time_to_sec(
		timediff(
			CAST(OpTime AS datetime),
			CAST(InOpTime AS datetime)
		)
	) / (60 * 60 * 24) < 1.5
	AND CAST(InOpTime AS datetime) < CAST(OpTime AS datetime)
	GROUP BY
		201407_1.InStationNo,
		201407_1.InRoadNo,
	  201407_1.StationNo,
	  201407_1.RoadNo
>
 
-  对收费站流量整合站id（num,insation,inroad,station,road）合并收费站的id得到（num,insation,inroad,station,road,inid,outid）
>	
	SELECT t1.num,t1.InStationNo,t1.InRoadNo,t1.StationNo,t1.RoadNo, t2.id,t3.id from `七月按三小时汇总流量` as t1
	left JOIN `收费站1` as t2
	on t1.InStationNo = t2.stationno and t1.InRoadNo = t2.station_roadno 
	LEFT JOIN  `收费站1` as t3
	on t1.StationNo  = t3.stationno AND t1.RoadNo = t3.station_roadno

- 
