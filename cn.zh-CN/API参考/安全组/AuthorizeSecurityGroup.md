# AuthorizeSecurityGroup {#doc_api_1031569 .reference}

增加一条安全组入方向规则。指定安全组入方向的访问权限，允许或者拒绝其他设备发送入方向流量到安全组里的实例。

## 接口说明 {#description .section}

本文档中，入方向流量的发起端为源端（Source），数据传输的接收端为目的端（Destination）。

调用该接口时，您需要了解：

-   一个安全组的安全组规则最多为 100 条。
-   安全组规则分为接受访问（accept）和拒绝访问（drop）两类。
-   安全组规则优先级（Priority）可选范围为 1~100。数字越小，代表优先级越高。
-   优先级相同的安全组规则，以拒绝访问（drop）的规则优先。
-   源端设备可以是指定 IP 地址范围（SourceCidrIp），也可以是其他安全组（SourceGroupId）中的实例。
-   以下任意一组参数可以确定一条安全组规则，只指定一个参数无法确定一条安全组规则。如果匹配的安全组规则已存在，此次 AuthorizeSecurityGroup 调用失败。
    -   设置指定 IP 地址段的访问权限。此时，经典网络类型安全组的网卡类型（NicType）可设置公网（internet）和 内网（intranet）。VPC 类型安全组的网卡类型（NicType）只可设置内网（intranet）。如以下请求示例：IpProtocol、PortRange、（可选）SourcePortRange、NicType、Policy、（可选）DestCiderIp 和 SourceCidrIp

        ```
        
                https://ecs.aliyuncs.com/?Action=AuthorizeSecurityGroup
                &SecurityGroupId=sg-F876FF7BA
                &SourceCidrIp=0.0.0.0/0
                &IpProtocol=tcp
                &PortRange=1/65535
                &NicType=intranet
                &Policy=accept
                &<公共请求参数>
                
        ```

        -   设置其他安全组的访问权限。此时，网卡类型（NicType）只能为内网（intranet）。经典网络类型安全组之间互访时，可以设置同一地域中其他安全组对您的安全组的访问权限。这个安全组可以是您的也可以是其他阿里云账户（SourceGroupOwnerAccount）的。VPC 类型安全组之间互访时，可以设置同一 VPC 内其他安全组访问该安全组的访问权限。如以下请求示例：IpProtocol、PortRange、（可选）SourcePortRange、NicType、Policy、（可选）DestCiderIp、SourceGroupOwnerAccount和 SourceGroupId

            ```
            
                    https://ecs.aliyuncs.com/?Action=AuthorizeSecurityGroup
                    &SecurityGroupId=sg-F876FF7BA
                    &SourceGroupId=sg-1651FBB64
                    &SourceGroupOwnerAccount=test@aliyun.com
                    &IpProtocol=tcp
                    &PortRange=1/65535
                    &NicType=intranet
                    &Policy=drop
                    &<公共请求参数>
                   
            ```

-   更多关于安全组规则的设置示例，请参阅 [应用案例](~~25475~~)、[安全组规则的典型应用](~~58746~~) 和 [安全组五元组规则介绍](~~97439~~)。

## 调试 {#apiExplorer .section}

前往【[API Explorer](https://api.aliyun.com/#product=Ecs&api=AuthorizeSecurityGroup)】在线调试，API Explorer 提供在线调用 API、动态生成 SDK Example 代码和快速检索接口等能力，能显著降低使用云 API 的难度，强烈推荐使用。

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|IpProtocol|String|是|all|传输层协议。取值大小写敏感。取值范围：

 -   tcp
-   udp
-   icmp
-   gre
-   all：支持所有协议

 |
|PortRange|String|是|1/200|SecurityGroupId 方开放的传输层协议相关的端口范围。取值范围：

 -   TCP/UDP 协议：取值范围为 1~65535 。使用斜线（/）隔开起始端口和终止端口。正确示范：1/200；错误示范：200/1。
-   ICMP 协议：-1/-1
-   GRE 协议：-1/-1
-   IpProtocol 取值为 all：-1/-1

 |
|RegionId|String|是|cn-hangzhou|安全组所属地域 ID。您可以调用 [DescribeRegions](~~25609~~) 查看最新的阿里云地域列表。

 |
|SecurityGroupId|String|是|sg-securitygroupid1|目的端安全组 ID。

 |
|Action|String|否|AuthorizeSecurityGroup|系统规定参数。取值：AuthorizeSecurityGroup

 |
|ClientToken|String|否|123e4567-e89b-12d3-a456-426655440000|保证请求幂等性。从您的客户端生成一个参数值，确保不同请求间该参数值唯一。**ClientToken** 只支持 ASCII 字符，且不能超过 64 个字符。更多详情，请参阅 [如何保证幂等性](~~25693~~)。

 |
|Description|String|否|FinanceJoshua|安全组规则的描述信息。长度为 1~512 个字符。

 |
|DestCidrIp|String|否|0.0.0.0/0|目的端 IPv4 CIDR 地址段。支持 CIDR 格式和 IPv4 格式的 IP 地址范围。 默认值：无。

 |
|Ipv6DestCidrIp|String|否|2001:db8:1234:1a00::XXX|目的端 IPv6 CIDR 地址段。支持 CIDR 格式和 IPv6 格式的 IP 地址范围。

 **说明：** 仅支持 VPC 类型的 IP 地址。

 默认值：无。

 |
|Ipv6SourceCidrIp|String|否|2001:db8:1234:1a00::XXX|源端 IPv6 CIDR 地址段。支持CIDR格式和IPv6格式的IP地址范围。

 **说明：** 仅支持 VPC 类型的 IP 地址。

 默认值：无。

 |
|NicType|String|否|intranet|网卡类型。取值范围：

 -   internet：公网网卡。
-   intranet：内网网卡。

 当设置安全组之间互相访问时，即指定了 SourceGroupId 且没有指定 SourceCidrIp 时，参数 NicType 取值只能为 intranet。 默认值：internet

 |
|OwnerAccount|String|否|ECSforCloud@Alibaba.com|RAM用户的账号登录名称。

 |
|Policy|String|否|accept|设置访问权限。取值范围：

 -   accept（默认）：接受访问。
-   drop：拒绝访问，不发回拒绝信息。

 |
|Priority|String|否|1|安全组规则优先级。取值范围：1~100 默认值：1

 |
|SourceCidrIp|String|否|0.0.0.0/0|源端 IPv4 CIDR 地址段。支持 CIDR 格式和 IPv4 格式的 IP 地址范围。 默认值：无。

 |
|SourceGroupId|String|否|sg-securitygroupid2|需要设置访问权限的源端安全组 ID。至少设置一项 SourceGroupId 或者 SourceCidrIp 参数。

 如果指定了 SourceGroupId 没有指定参数 SourceCidrIp，则参数 NicType 取值只能为 intranet。

 如果同时指定了 SourceGroupId 和 SourceCidrIp，则默认以 SourceCidrIp 为准。

 |
|SourceGroupOwnerAccount|String|否|test@aliyun.com|跨账户设置安全组规则时，源端安全组所属的阿里云账户。

 -   如果 SourceGroupOwnerAccount 及 SourceGroupOwnerID 均未设置，则认为是设置您其他安全组的访问权限。
-   如果已经设置参数 SourceCidrIp，则参数 SourceGroupOwnerAccount 无效。

 |
|SourceGroupOwnerId|Long|否|155780923770|跨账户设置安全组规则时，源端安全组所属的阿里云账户 ID。

 -   如果 SourceGroupOwnerId 及 SourceGroupOwnerAccount 均未设置，则认为是设置您其他安全组的访问权限。
-   如果您已经设置参数 SourceCidrIp，则参数 SourceGroupOwnerId 无效。

 |
|SourcePortRange|String|否|1/200|源端安全组开放的传输层协议相关的端口范围。取值范围：

 -   TCP/UDP 协议：取值范围为 1~65535。使用斜线（/）隔开起始端口和终止端口。正确示范：1/200；错误示范：200/1。
-   ICMP 协议：-1/-1。
-   GRE 协议：-1/-1。
-   IpProtocol 取值为 all：-1/-1

 |

## 返回参数 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E|请求 ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

https://ecs.aliyuncs.com/?Action=AuthorizeSecurityGroup
&SecurityGroupId=sg-F876FF7BA
&SourceCidrIp=0.0.0.0/0
&IpProtocol=tcp
&PortRange=1/65535
&NicType=intranet
&Policy=accept
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<AuthorizeSecurityGroupResponse>
  <RequestId>CEF72CEB-54B6-4AE8-B225-F876FF7BA984</RequestId>
</AuthorizeSecurityGroupResponse>

```

`JSON` 格式

``` {#json_return_success_demo}
{
	"RequestId":"CEF72CEB-54B6-4AE8-B225-F876FF7BA984"
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|404|InvalidSecurityGroupId.NotFound|The specified SecurityGroupId does not exist.|指定的安全组在该用户账号下不存在，请您检查安全组id是否正确。|
|400|OperationDenied|The specified IpProtocol does not exist or IpProtocol and PortRange do not match.|指定的 IP 协议不存在，或与端口范围不匹配。|
|400|InvalidIpProtocol.Malformed|The specified parameter "PortRange" is not valid.|IP协议参数格式不正确。|
|403|MissingParameter|The input parameter "SourceGroupId" or "SourceCidrIp" cannot be both blank.|源ip或者源安全组不能同时为空。|
|400|InvalidPolicy.Malformed|The specified parameter "Policy" is not valid.|指定的参数无效，请您检查该参数是否正确。|
|400|InvalidNicType.Mismatch|Specified nic type conflicts with the authorization record.|指定的 NIC 类型不存在。|
|403|InvalidParamter.Conflict|The specified SecurityGroupId should be different from the SourceGroupId.|授权与被授权安全组必须不同|
|400|InvalidPriority.Malformed|The parameter Priority is invalid.|无效的规则优先级。|
|400|InvalidPriority.ValueNotSupported|The parameter Priority is invalid.|无效的规则优先级。|
|403|InvalidNetworkType.Conflict|The specified SecurityGroup network type should be same with SourceGroup network type \(vpc or classic\).|指定的 SecurityGroup 的网络类型必须与 SouceGroup 的网络类型一致。|
|400|InvalidSecurityGroupDiscription.Malformed|The specified security group rule description is not valid.|指定的安全组规则描述不合法。|
|400|InvalidSecurityGroup.InvalidNetworkType|The specified security group network type is not support this operation, please check the security group network types. For VPC security groups, ClassicLink must be enabled.|无效的网络类型。|
|400|MissingParameter.Source|Either SourceCidrIp or SourceGroupId must be specified.|源ip或者源安全组不能同时为空。|
|400|InvalidParam.PortRange|Please specify the PortRange or SourcePortRange in integer, less than 65535, and separate the range with ?/?.|请指定目的端口范围或者源端口范围，端口号要小于65535，如果是端口范围需要使用"/"分隔。|
|400|InvalidIpProtocol.ValueNotSupported|The parameter IpProtocol must be specified with case insensitive TCP, UDP, ICMP, GRE or All.|协议类型只能是TCP， UDP， ICMP， GRE 或者 All。|
|400|InvalidParam.SourceIp|%s|无效的源IP。|
|400|InvalidParam.DestIp|%s|无效的目的IP。|
|400|InvalidParam.Ipv6DestCidrIp|%s|无效的目的IP\(v6格式\)。|
|400|InvalidParam.Ipv6SourceCidrIp|%s|无效的源IP\(v6格式\)。|
|400|InvalidParam.Ipv4ProtocolConflictWithIpv6Address|%s|IPv4和IPv6不能同时存在。|
|400|InvalidParam.Ipv6ProtocolConflictWithIpv4Address|%s|IPv6和IPv4不能同时存在。|
|400|ILLEGAL\_IPV6\_CIDR|%s|指定的IPv6地址段无效。|

[查看本产品错误码](https://error-center.aliyun.com/status/product/Ecs)

