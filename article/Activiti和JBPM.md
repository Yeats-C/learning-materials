# Activiti和JBPM

## 概念

在 Java 中，Activiti 和 jBPM 都是开源的工作流引擎，用来实现业务流程管理 (BPM)，它们都支持 BPMN 2.0 标准，但在架构、技术栈和应用场景上有所不同。 Activiti 更轻量、易集成，适合快速开发；jBPM 功能更全面，依赖 JBoss 技术生态，适合大型企业系统。

Activiti  
由 Tom Baeyens 在离开 JBoss 后创建，基于 jBPM3/4 的经验发展而来。强调轻量化、易用性和与 Spring 的集成。

jBPM  
全称 Java Business Process Management，由 JBoss 社区维护，功能覆盖工作流、规则引擎、服务编排，依赖 Drools Flow 和 JBoss 技术栈。

## 特性


特性	                      Activiti	                                    jBPM
架构基础	                    基于 PVM (流程虚拟机)，延续 jBPM3/4 思路	      基于 Drools Flow，重构 jBPM5
持久化	                      使用 MyBatis，未遵循 JPA 标准	                使用 Hibernate，遵循 JPA/JTA
集成能力	                    与 Spring、CXF、Mule ESB 集成方便	            与 JBoss、Drools、Guvnor 深度绑定
人机交互任务	                内置支持，API 简洁	                            使用 WS-HT (WebService Human Task) 标准
优势	                      上手快、轻量、社区活跃	                        功能全面、事务标准化、RedHat 支持
劣势	                      持久化不够标准化	                              技术依赖过重、学习曲线较陡

## 应用场景

Activiti 应用

适合中小型企业快速构建审批流、流程自动化。

常用于 OA 系统、轻量级 BPM 平台。

优点是 易集成、学习成本低。

jBPM 应用

更适合大型企业系统，尤其是依赖 JBoss 技术栈的环境。

常用于金融、电信、政府等对流程管理要求严格的场景。

优点是 功能全面、事务和规则引擎支持强。

## 总结

Activiti：如果团队熟悉 Spring，追求快速开发和灵活集成，Activiti 更合适。

jBPM：如果企业已有 JBoss/Drools 技术栈，且需要更复杂的流程编排和规则管理，jBPM 更合适。

共同点：两者都支持 BPMN 2.0，都是开源项目，社区活跃，但需要根据 项目规模、技术栈和团队熟悉度 来选择。
