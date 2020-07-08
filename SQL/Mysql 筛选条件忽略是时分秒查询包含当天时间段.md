<if test="beginTime!= null">
    and a.begin_time &gt;= DATE_FORMAT(#{beginTime},'%Y-%m-%d 00:00:00')
</if>
<if test="finishTime!= null">
    and a.finish_time &lt;= DATE_FORMAT(#{finishTime},'%Y-%m-%d 23:59:59')
</if>
