---
title: 使用自定义策略管理 SSO 和令牌自定义
titleSuffix: Azure AD B2C
description: 了解在 Azure Active Directory B2C 中使用自定义策略管理 SSO 和令牌自定义。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e72bd04bb41537546191b8ceb320c0722bd10146
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92340285"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中使用自定义策略管理 SSO 和令牌自定义

本文提供了有关在 Azure Active Directory B2C (Azure AD B2C) 中如何使用[自定义策略](custom-policy-overview.md)管理令牌、会话和单一登录 (SSO) 配置的信息。

## <a name="jwt-token-lifetimes-and-claims-configuration"></a>JWT 令牌生存期和声明配置

若要更改令牌生存期的设置，需要在想要影响的策略的信赖方文件中添加 [ClaimsProviders](claimsproviders.md) 元素。  **ClaimsProviders** 元素是 [TrustFrameworkPolicy](trustframeworkpolicy.md) 元素的子代。

在信赖方文件的 BasePolicy 元素和 RelyingParty 元素之间插入 ClaimsProviders 元素。

在该元素中，需要放置影响令牌生存期的信息。 XML 如以下示例所示：

```xml
<ClaimsProviders>
  <ClaimsProvider>
    <DisplayName>Token Issuer</DisplayName>
    <TechnicalProfiles>
      <TechnicalProfile Id="JwtIssuer">
        <Metadata>
          <Item Key="token_lifetime_secs">3600</Item>
          <Item Key="id_token_lifetime_secs">3600</Item>
          <Item Key="refresh_token_lifetime_secs">1209600</Item>
          <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
          <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
          <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
        </Metadata>
      </TechnicalProfile>
    </TechnicalProfiles>
  </ClaimsProvider>
</ClaimsProviders>
```

上一示例中设置了以下值：

- **访问令牌生存期** - 访问令牌生存期值是通过 **token_lifetime_secs** 元数据项设置的。 默认值为 3600 秒（60 分钟）。
- **ID 令牌生存期** - ID 令牌生存期值是通过 **id_token_lifetime_secs** 元数据项设置的。 默认值为 3600 秒（60 分钟）。
- **刷新令牌生存期** - 刷新令牌生存期值是通过 **refresh_token_lifetime_secs** 元数据项设置的。 默认值为 1209600 秒（14 天）。
- **刷新令牌滑动窗口生存期** - 若要为刷新令牌设置滑动窗口生存期，请设置 **rolling_refresh_token_lifetime_secs** 元数据项的值。 默认值为 7776000（90 天）。 如果不想强制实施滑动窗口生存期，请将此项替换为 `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`。
- **颁发者 (iss) 声明** - 颁发者 (iss) 声明是通过 **IssuanceClaimPattern** 元数据项设置的。 适用的值为 `AuthorityAndTenantGuid` 和 `AuthorityWithTfp`。
- **设置声明表示策略 ID** - 用于设置此值的选项为 `TFP`（信任框架策略）和 `ACR`（身份验证上下文引用）。 `TFP` 是建议使用的值。 将 **AuthenticationContextReferenceClaimPattern** 设置为值 `None`。

    在 ClaimsSchema 元素中，添加此元素：

    ```xml
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```

    在 OutputClaims 元素中，添加此元素：

    ```xml
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    对于 ACR，请删除 **AuthenticationContextReferenceClaimPattern** 项。

- **使用者 (sub) 声明** - 此选项默认为 ObjectID，如果要将此设置切换为 `Not Supported`，请替换以下行：

    ```xml
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```

    替换为以下代码行：

    ```xml
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

> [!NOTE]
> 使用授权代码流和 PKCE 的单页面应用程序的刷新令牌生存期始终为24小时。 [详细了解浏览器中刷新令牌的安全隐患](../active-directory/develop/reference-third-party-cookies-spas.md#security-implications-of-refresh-tokens-in-the-browser)。

## <a name="provide-optional-claims-to-your-app"></a>向应用程序提供可选声明

[信赖方策略技术配置文件](relyingparty.md#technicalprofile)输出声明是返回到应用程序的值。 添加输出声明会在用户成功旅程后向令牌发出声明，并将其发送到应用程序。 修改 "信赖方" 部分中的 "技术配置文件" 元素，以将所需声明添加为输出声明。

1. 打开自定义策略文件。 例如，SignUpOrSignin.xml。
1. 查找 OutputClaims 元素。 添加要包含在令牌中的 OutputClaim。 
1. 设置输出声明特性。 

下面的示例添加 `accountBalance` 声明。 AccountBalance 声明将作为平衡发送到应用程序。 

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
      <!--Add the optional claims here-->
      <OutputClaim ClaimTypeReferenceId="accountBalance" DefaultValue="" PartnerClaimType="balance" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

OutputClaim 元素包含以下属性：

  - **ClaimTypeReferenceId** -策略文件或父策略文件中 [ClaimsSchema](claimsschema.md) 部分定义的声明类型的标识符。
  - **PartnerClaimType** -允许在令牌中更改声明的名称。 
  - **DefaultValue** -默认值。 你还可以将默认值设置为 [声明解析程序](claim-resolver-overview.md)，如租户 ID。
  - **AlwaysUseDefaultValue** -强制使用默认值。

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure AD B2C 会话](session-overview.md)。
- 了解如何[在自定义策略中配置会话行为](session-behavior-custom-policy.md)。
- 参考：[JwtIssuer](jwt-issuer-technical-profile.md)。
