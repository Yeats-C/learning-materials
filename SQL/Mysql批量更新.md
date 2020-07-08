Mysql批量更新的一个坑-&allowMultiQueries=true允许批量更新

前言
        实际上，我们经常会遇到这样的需求，那就是利用Mybatis批量更新或者批量插入，但是，实际上即使Mybatis完美支持你的sql，你也得看看你说操作的数据库是否支持，而阿福，最近就遇到这样的一个坑。

问题
        先带大家来看一段sql的配置，
<update id="updateAll" parameterType="java.util.List">
    <foreach collection="list" separator=";" open="" close="" index="index" item="item">
        update store_info
        set
        update_time= now()
        <if test="item.profitLoss!=null">,profit_loss = #{item.profitLoss}</if>
        <if test="item.categoryCode!=null">,category_code = #{item.categoryCode,jdbcType=VARCHAR}</if>
        <if test="item.salesLevelCode!=null">,sales_level_code = #{item.salesLevelCode,jdbcType=VARCHAR}</if>
        <if test="item.openDate!=null">,open_date = #{item.openDate,jdbcType=VARCHAR}</if>
        <if test="item.averageOrder!=null">,average_order = #{item.averageOrder,jdbcType=BIT}</if>
        <if test="item.averageSales!=null">,average_sales = #{item.averageSales,jdbcType=BIT}</if>
        <if test="item.rent!=null">,rent = #{item.rent,jdbcType=BIT}</if>
        <if test="item.grossProfitRate!=null">,gross_profit_rate = #{item.grossProfitRate,jdbcType=VARCHAR}</if>
        <if test="item.memberCount!=null">,member_count = #{item.memberCount,jdbcType=VARCHAR}</if>
        <if test="item.isWater!=null">,is_water = #{item.isWater,jdbcType=BIT}</if>
        <if test="item.createPersonId!=null">,create_person_id = #{item.createPersonId,jdbcType=VARCHAR}</if>
        <if test="item.createPersonName!=null">,create_person_name = #{item.createPersonName,jdbcType=VARCHAR}</if>
        <if test="item.userStatus!=null">,user_status = #{item.userStatus,jdbcType=BIT}</if>
        <if test="item.contractArea!=null">,contract_area = #{item.contractArea,jdbcType=BIT}</if>
        <if test="item.retailArea!=null">,retail_area = #{item.retailArea,jdbcType=BIT}</if>
        <if test="item.stockArea!=null">,stock_area = #{item.stockArea,jdbcType=BIT}</if>
        <if test="item.rentSellRatio!=null">,rent_sell_ratio = #{item.rentSellRatio,jdbcType=BIGINT}</if>
        <if test="item.originStoreId!=null">,origin_store_id = #{item.originStoreId,jdbcType=VARCHAR}</if>
        <if test="item.storeCode!=null">,store_code = #{item.storeCode,jdbcType=VARCHAR}</if>
        <if test="item.storeName!=null">,store_name = #{item.storeName,jdbcType=VARCHAR}</if>
        <if test="item.monthCount!=null">,month_count = #{item.monthCount,jdbcType=BIGINT}</if>
        <if test="item.waterArea!=null">,water_area = #{item.waterArea,jdbcType=BIGINT}</if>
        <if test="item.storeAddressStatus!=null">,store_address_status = #{item.storeAddressStatus,jdbcType=VARCHAR}</if>
        <if test="item.natureCode!=null">,nature_code = #{item.natureCode,jdbcType=VARCHAR}</if>
        <if test="item.createTime!=null">,create_time = #{item.createTime,jdbcType=VARCHAR}</if>
        where store_id=#{item.storeId,jdbcType=VARCHAR}
    </foreach>
</update>

看似似乎没有一点问题，这里用到了Mybatis的动态sql，实际上说白了也就是拼sql，不过这个繁杂的工作交给Mybatis帮我们去做了。可是，只要一执行就要报语法错误,org.springframework.jdbc.BadSqlGrammarException。

调试了好久，发现只要传一个值进去就没有问题，就是list的成员只有一个。

解决方案
        后来发现，原来mysql的批量更新是要我们主动去设置的， 就是在数据库的连接url上设置一下，加上* &allowMultiQueries=true *即可。
