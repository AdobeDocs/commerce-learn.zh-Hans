---
title: Adobe Commerce Cloud启动前核对清单
description: 了解Adobe Commerce Cloud启动前核对清单。
feature: Cloud
topic: Commerce, Architecture, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1668'
ht-degree: 0%

---

# Commerce Cloud启动前核对清单

以下是Adobe Commerce [站点启动文档](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}的概要。

此核对清单旨在协助规划和成功启动Adobe Commerce Cloud网站。 与您的Adobe Commerce Cloud系统集成商协作，确保完成并验证所有配置任务和清单项目。 如果您在使用任何清单项目时遇到困难或有任何疑问，请联系指定的客户技术顾问或客户成功工程师。 如果您的帐户没有分配的CTA/CSE，您可以创建支持工单以获得帮助。

如果您已为该帐户分配CTA/CSE，请在启动新的Adobe Commerce Cloud网站至少4周前联系他们和客户经理，以通知他们您的&#x200B;**启动意图**。

- 某些检查以[!BADGE Blocker]{type=caution tooltip="潜在的阻断因素"}突出显示，因为如果不仔细检查，这些检查可能会阻止您的上线。
- 确保与开发人员或系统集成合作伙伴协作，以符合您的实施方法。

>[!IMPORTANT]
> 如果您未使用并完成此核对清单，则您接受[责任](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}，因为这会给生产启动时间表和持续性站点稳定性带来任何不良影响和相关风险。

## 1.上线前

1. 查看有关测试和上线的文档[站点启动文档](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >确保与您的合作伙伴或系统集成商一起充分准备全面的&#x200B;_“上线准备计划”_，并纳入所有必要的行动项目。 请记住，尽管启动前核对清单强调Adobe的最佳实践，但&#x200B;_&#x200B;**并不**&#x200B;_&#x200B;取代您自己的上线准备计划的需要。

2. [!BADGE 阻止程序]{type=caution tooltip="潜在的阻断因素"}查看支持分析(SWAT)建议和信息（[用户指南](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"}）
3. 最终用户/商家执行UAT（用户验收测试），包括后端操作。
4. 系统集成商团队在暂存和生产环境中执行了端到端的UAT。 请参阅[Experience League文档](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}。
5. 确认在暂存和生产环境中部署代码并进行测试（[了解更多](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}）。
6. 生产群集的规模已永久增大到合同规定的每日基准。 与分配的CTA/CSE联系以了解更多详细信息，或提出支持票证。

## 2.当前配置

1. 将Adobe Commerce和相关包/服务升级到[最新版本](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/release/notes/overview){target="_blank"}
2. 与您的SI/合作伙伴一起查看当前的配置和服务，然后[遵循最佳实践](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}。
3. 查看MySQL/共享文件[磁盘使用情况](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## &#x200B;3. Fastly配置

1. [!BADGE 阻止程序]{type=caution tooltip="潜在的阻断因素"}确保缓存正常工作([全页缓存](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"}或[GraphQL缓存](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"})。 阅读[快速设置指南](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}。
2. 适用时，在PWA/Headless网站上使用GET方法进行GraphQL查询。

   >[!NOTE]
   > 只能缓存通过HTTP GET操作提交的查询（如果适用）。 无法缓存[POST查询](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}。

3. 确保启用Fastly图像优化（[请参阅Fastly图像优化](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"}）
4. 验证是否配置了正确的屏蔽位置（[配置缓存、后端和源屏蔽](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}）。
5. Web应用程序防火墙(**WAF**)正在工作。 (请参阅[对阻止的请求进行故障排除](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}（如果有）和限制)
6. 更新管理面板中的Fastly [“忽略的URL参数”](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"}列表以提高缓存性能。

   >[!NOTE]
   > 在&#x200B;_管理员>存储>配置>系统>全页缓存> Fastly配置>高级配置>忽略的URL参数（全局）_&#x200B;下的Fastly配置中，您可以找到Fastly在搜索缓存页面时应忽略的逗号分隔参数列表。 请确保在修改此列表后重新上传VCL

## &#x200B;4. DNS和SSL

1. [!BADGE 阻止程序]{type=caution tooltip="潜在的阻断因素"}确认请求了所有必需的域名。 _（为所有添加或更改的域提前提交支持票证）_
2. [!BADGE 阻止程序]{type=caution tooltip="潜在的阻断因素"} SSL (TLS)证书已应用于域。 有关详细信息，请阅读[本文](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"}。
3. 将DNS [TTL （生存时间）](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"}值更新到最小值，以便上线。
4. 启用Sendgrid SPF和DKIM

   >[!NOTE]
   > 将每个域的SendGrid CNAME记录添加到DNS配置中。 阅读[SendGrid电子邮件服务](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}以了解如何更改发件人域等。

## 5.数据库配置

Adobe Commerce Cloud使用MariaDB Galera群集作为暂存环境和生产环境的数据库。 Galera群集有助于提高性能和可扩展性。 要深入了解Galera群集复制的最佳实践和约束条件，请参阅以下文章。

- [MySQL配置最佳实践](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Adobe Commerce上的托管警报： [MariaDB警报](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- [数据库配置](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}的最佳实践
- 对[Galera群集复制和流量控制的深入分析。](https://experienceleague.adobe.com/zh-hans/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. 建议使用[MYSQL从属连接](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"}以提高数据库高负载期间的性能。
2. 确保所有数据库表的行格式都设置为[DYNAMIC而不是COMPACT](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} （对于本地到云的迁移尤其如此）。
3. 将所有表的数据库存储引擎从[MyISAM更改为InnoDB](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"}。
4. 提前审查并优化大小超过1 GB的数据库表。
5. 数据库架构信息是最新的信息。 （请参阅[本指南](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}）。

## 6.部署

1. 查看静态内容部署(SCD)理想状态，以减少在生产环境中进行部署期间的维护时间。 查看[静态内容部署(SCD)策略](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"}和[存储配置管理](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"}指南。
2. 查看HTML、JavaScript和CSS的缩小设置。 (这不适用于PWA/Headless网站)。
3. 确认以下云变量的使用符合其预期目的。 （[SCD_MATRIX](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}、[SCD_ON_DEMAND](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"}和[SKIP_SCD](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"}）

## 7.测试和故障排除

1. 测试传出事务性电子邮件。 详细了解[Adobe Commerce Cloud - SendGrid Mail功能](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}。
2. [!BADGE 阻止程序]{type=caution tooltip="潜在的阻断因素"}Adobe中是否有阻止程序？
3. [!BADGE 阻止程序]{type=caution tooltip="潜在的阻断因素"}在生产实例上线之前执行负载和压力测试，并与分配的CTA/CSE共享结果。

   >[!NOTE]
   > [负载和压力测试用于识别应用程序中的瓶颈并发现性能问题](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"}。 它在管理对集群规模的期望和确定必要的扩展调整以有效满足业务需求方面发挥着关键作用。

   >[!IMPORTANT]
   > **_WARNING:_**&#x200B;在准备负载测试时，请_ **_不要_**&#x200B;发送实时交易电子邮件（甚至发送到虚拟地址）。 在测试期间发送电子邮件可能导致项目达到在启动之前为SendGrid配置的默认发送限制(12k)。
   > 
   > - 如何禁用电子邮件通信：
   >   转到&#x200B;_商店>配置>高级>系统>电子邮件发送设置_。

4. 作为[责任共享安全模型](https://business.adobe.com/cn/products/magento/secure-ecommerce.html){target="_blank"}的一部分，对生产实例进行安全渗透测试。 为符合PCI（支付卡行业）标准，定制站点需要进行渗透测试。

## 8.其他配置

1. 将索引切换为&#x200B;_&quot;计划_&#x200B;更新&quot;，但&#x200B;**_customer_grid_**&#x200B;保留在&quot;SAVE&quot;中（请参阅[索引模式](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}）。
2. 您是否使用任何第三方搜索引擎或扩展？
3. 确认已正确设置[SEO（搜索引擎优化）配置](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"}以启用索引器/爬网程序扫描网站（如果相关）。
4. 添加重定向和路由（请参阅[配置路由](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"}）

   >[!NOTE]
   >在集成环境中将重定向和路由添加到routes.yaml文件，并在部署到暂存和生产环境之前验证此环境中的配置。

       &quot;http://{all}/&quot;：
       类型：上游
       上游： &quot;mymagento：http&quot;
       
       &quot;http://{all}/&quot;：
       类型：上游
       上游：&quot;mymagento：http&quot;
   
5. 如果在开发期间启用，请确保禁用XDebug（请参阅[配置Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}）。
6. 验证是否已在php.ini文件中准确更新了操作缓存和其他配置（[请参考此示例](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}）。
7. 订阅&#x200B;[**Adobe Commerce状态页**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}。
8. 订阅New Relic针对Adobe Commerce[通知渠道的](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}托管警报，以监视给定的性能指标（[了解更多](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}）。

## 9.安全

1. 设置Adobe Commerce安全扫描

   >[!NOTE]
   > [Adobe Commerce安全扫描是一种有用的工具](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/security/security-scan){target="_blank"}，可帮助发现网站上的过时软件版本、不正确的配置和潜在的恶意软件。 注册、安排其经常运行，并确保将电子邮件发送给正确的技术安全联系人。
   > 
   > 在UAT期间完成此任务。 如果使用定期扫描选项，请确保在低需求时间安排扫描。 查看Adobe Commerce帐户中的[安全扫描](https://account.magento.com/scanner/index/dashboard/){target="_blank"}页面。 您必须登录Adobe Commerce帐户才能访问安全扫描。

2. 更改Adobe Commerce管理员的默认设置。
3. 更改管理员密码（请参阅[配置管理员安全](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/security/security-admin){target="_blank"}）。
4. 更改管理员URL（请参阅[使用自定义管理员URL](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}）。
5. 删除项目中不再存在的所有用户（请参阅[创建和管理用户](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}）。
6. 管理员密码已配置（请参阅[管理员密码要求](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/security/security-admin){target="_blank"}）。
7. 配置双重身份验证（请参阅[双重身份验证](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}）。

## 10.上线

当需要进行转换时，请执行以下步骤（有关详细信息，请参阅[DNS配置](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}）：

1. 访问您的DNS服务并更新每个域和主机名的A和CNAME记录：
   1. 添加&#x200B;_&lt;&lt;www.yourdomain.com>>_&#x200B;的CNAME记录，指向&#x200B;**prod.magentocloud.map.fastly.net**
   2. 为&#x200B;_&lt;&lt;yourdomain.com>>_&#x200B;设置四条A记录，指向：\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. 将Adobe Commerce基本URL更改为&#x200B;_&lt;&lt;www.yourdomain.com>>_
3. 等待TTL时间过去，然后重新启动Web浏览器。
4. 测试网站。

### 如果您遇到阻止上线的问题：

如果遇到任何问题，导致您在直接转换期间无法启动商店，则获得适当及时支持的最快方法是：利用技术支持并打开票证（原因是“无法启动我的商店”），然后调用热线支持号码(请参阅[Adobe Commerce P1（优先级1）热线号码列表](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"})：

- 美国免费电话： (+1) 877 282 7436(直接到Adobe Commerce P1热线)
- 美国免费电话：(+1) 800 685 3620 (第一个菜单，按7访问Adobe Commerce P1热线)
- 美国当地： (+1) 408 537 8777

## 11.上线后

在站点上线后，发送电子邮件给分配的CTA（客户技术咨询）、CSE（客户成功工程师）和AM（客户经理）。 但是，如果您没有为该项目分配客户经理，则可以创建一个支持工单，要求在站点上线后启用高SLA监控。 一旦验证站点启动时启用了Fastly和缓存，CTA/CSE就会执行以下任务：

- 将群集标记为实时并创建支持工单以激活高SLA（服务水平协议）监控。
- 激活New Relic Synthetics以监控正常运行时间。

>[!MORELIKETHIS]
> 
> - [启动准备概述 — 实施行动手册](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [启动项核对清单 — 用户指南](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [启动前核对清单 — 站点管理员/Commerce管理员指南](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [共享责任安全模型](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
