# java数据库动态配置的定时任务
* 数据库创建
```
/*
 Navicat Premium Data Transfer

 Source Server         : 192.168.199.150
 Source Server Type    : MySQL
 Source Server Version : 80023
 Source Host           : 192.168.199.150:3306
 Source Schema         : pro_public

 Target Server Type    : MySQL
 Target Server Version : 80023
 File Encoding         : 65001

 Date: 25/06/2021 17:14:20
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for public_timing_task
-- ----------------------------
DROP TABLE IF EXISTS `public_timing_task`;
CREATE TABLE `public_timing_task`  (
  `seq_id` bigint NOT NULL AUTO_INCREMENT COMMENT '自动增长（PK）',
  `event_name` varchar(200) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '执行事件名',
  `event_at_every` varchar(10) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT 'EVERY' COMMENT '执行频率（AT一次执行EVERY循环执行）',
  `starts_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '开始执行时间（年月日时分）',
  `event_time` int NOT NULL DEFAULT 0 COMMENT '执行时间',
  `e_interval` varchar(10) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '时间间隔（MINUTE,HOUR,DAY,MONTH,YEAR）',
  `rollback_interval` int NOT NULL DEFAULT 0 COMMENT '回滚间隔（为0不回滚，不为0时在执行失败后，多少分钟进行回滚1次）',
  `comment` varchar(200) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '注释',
  `do_call` varchar(200) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '执行命令',
  `do_call_time` varchar(200) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '服务定时任务时间配置',
  `status` tinyint(1) NOT NULL DEFAULT 1 COMMENT '状态(0禁用/1启用)',
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  `standby1` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '备用字段1',
  `standby2` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '备用字段2',
  `standby3` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' COMMENT '备用字段3',
  PRIMARY KEY (`seq_id`) USING BTREE,
  UNIQUE INDEX `AK_UK_1`(`event_name`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 4 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '定时任务（数据归类）' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of public_timing_task
-- ----------------------------
INSERT INTO `public_timing_task` VALUES (1, '行业供需数据归档', 'EVERY', '2021-06-10 20:10:00', 1, 'MONTH', 10, '每月10日晚上20:10执行一次更新2个月数据', 'call pr_industry_cg_extracting();', '0 10 20 10 * ?', 1, '2021-05-16 19:05:21', '2021-06-25 17:13:39', '0 10 20 10 * ?', '', '');
INSERT INTO `public_timing_task` VALUES (2, '天气数据归类', 'EVERY', '2021-05-13 10:00:00', 30, 'MINUTE', 10, '30分钟执行一次气象数据抽取', 'call pr_weather_extracting();', '0 0/30 * * * ?', 0, '2021-05-16 19:10:05', '2021-06-25 14:54:33', '0 0/30 * * * ?', '', '');
INSERT INTO `public_timing_task` VALUES (3, '数据抽取全国天然气供气量走势', 'EVERY', '2021-06-10 20:30:00', 1, 'MONTH', 10, '每月10日晚上20:30执行一次抽取上月数据', 'call pr_industry_cg_extracting();', '0 30 20 10 * ?', 1, '2021-05-17 20:34:08', '2021-06-25 14:54:33', '0 30 20 10 * ?', '', '');

SET FOREIGN_KEY_CHECKS = 1;

```

* 代码
```
package cn.enn.ssd.commonToolService.controller;

import java.util.Date;
import java.util.List;
import java.util.Map;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.SchedulingConfigurer;
import org.springframework.scheduling.config.ScheduledTaskRegistrar;
import org.springframework.scheduling.support.CronTrigger;
import org.springframework.stereotype.Component;

/**
 * @description: 数据库动态配置的定时任务
 * @author: chensifang
 * @create_date 2021年06月25日
 */
@Component
@Configuration //1.主要用于标记配置类，兼备Component的效果。
@EnableScheduling // 2.开启定时任务
public class DataExtractionJobs implements SchedulingConfigurer {

  private static final Logger logger = LoggerFactory
      .getLogger(DataExtractionJobs.class);

  @Autowired
  private JdbcTemplate jdbcTemplate;




  /**
   * 执行定时任务.
   */

  @Override
  public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
    String sql = "SELECT event_name,event_at_every,starts_time,event_time,e_interval,rollback_interval,comment,do_call,status,create_time,update_time,standby1 from pro_public.public_timing_task where status=1 ";
    jdbcTemplate.setQueryTimeout(25000);
    List list = jdbcTemplate.queryForList(sql);
    if (null != list && list.size() > 0) {
      for (int i = 0; i < list.size(); i++) {
        Map cronMap = (Map) list.get(i);
        String do_call = (String) cronMap.get("do_call");
        String cron = (String) cronMap.get("standby1");
        String event_name = (String) cronMap.get("event_name");

        taskRegistrar.addTriggerTask(
            //1.添加任务内容(Runnable)
            () -> System.out.println("执行动态定时任务: " + event_name + new Date()),
            //2.设置执行周期(Trigger)
            triggerContext -> {
              //2.1 从数据库获取执行周期
              implementationPlan(do_call);
//              System.out.println("执行动态定时任务+1111: " + LocalDateTime.now().toLocalTime());
              //2.2 合法性校验.
              // Omitted Code ..
              //2.3 返回执行周期(Date)
              return new CronTrigger(cron).nextExecutionTime(triggerContext);
            }
        );
      }
    }
  }

  private void implementationPlan(String sql) {
    System.out.println("sql=" + sql);
    jdbcTemplate.setQueryTimeout(25000);
//    jdbcTemplate.execute(sql);
  }


}

```
