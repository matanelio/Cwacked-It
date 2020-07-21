<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper PUBLIC "-//ibatis.apache.org//DTD Mapper 3.0//EN"      
 "http://ibatis.apache.org/dtd/ibatis-3-mapper.dtd">
<mapper namespace="com.wit.dao.PasswordUnlockerDao">
	<!-- 查询所有 -->
	<select id="getAll" resultType="com.wit.model.PasswordUnlocker">
		select
		id,userId,verifiCode,verifiDate,status
		from PasswordUnlocker
	</select>
	<!-- 删除 -->
	<delete id="delete" parameterType="Integer">
		delete from PasswordUnlocker
		where id=#{id}
	</delete>
	<!-- 添加 -->
	<insert id="add" parameterType="hashMap">
		<selectKey keyProperty="id" resultType="Integer" order="AFTER">
			SELECT LAST_INSERT_ID()
		</selectKey>
		<if test="userId==null">
			insert into PasswordUnlocker(verifiCode,verifiDate,status)
			values(#{SendContent},NOW(),0)
		</if>
		<if test="userId!=null">
			insert into
			PasswordUnlocker(userId,verifiCode,verifiDate,status)
			values(#{userId},#{SendContent},NOW(),0)
		</if>
	</insert>
	<!--根据Id查询 -->
	<select id="findbyId" parameterType="Integer"
		resultType="com.wit.model.PasswordUnlocker">
		select id,userId,verifiCode,verifiDate,status
		from
		PasswordUnlocker
		where id=#{id}
	</select>
	<!-- 修改 -->
	<update id="update" parameterType="com.wit.model.PasswordUnlocker">
		update PasswordUnlocker set
		userId=#{userId},verifiCode=#{verifiCode},
		verifiDate=#{verifiDate},status=#{status}
		where id=#{id}
	</update>

	<!-- 根据用户ID查询验证码信息 -->
	<select id="findByUserId" parameterType="Integer"
		resultType="com.wit.model.PasswordUnlocker">
		select id,userId,verifiCode,verifiDate,status
		from
		PasswordUnlocker
		where userId=#{userId} AND id=(SELECT MAX(id) from
		PasswordUnlocker)
	</select>



</mapper>
