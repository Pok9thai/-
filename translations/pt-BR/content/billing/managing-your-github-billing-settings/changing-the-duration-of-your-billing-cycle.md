---
title: Alterar a duração do ciclo de cobrança
intro: 'Você pode pagar pela assinatura da sua conta, bem como de outros recursos e produtos pagos em um ciclo de cobrança mensal ou anual.'
redirect_from:
  - /github/setting-up-and-managing-billing-and-payments-on-github/changing-the-duration-of-your-billing-cycle
  - /articles/monthly-and-yearly-billing/
  - /articles/switching-between-monthly-and-yearly-billing-for-your-personal-account/
  - /articles/switching-between-monthly-and-yearly-billing-for-your-organization/
  - /articles/changing-the-duration-of-your-billing-cycle
  - /github/setting-up-and-managing-billing-and-payments-on-github/managing-your-github-billing-settings/changing-the-duration-of-your-billing-cycle
versions:
  fpt: '*'
  ghec: '*'
type: how_to
topics:
  - Organizations
  - Repositories
  - User account
shortTitle: Ciclo de cobrança
---

Quando você altera a duração do ciclo de cobrança, sua assinatura do {% data variables.product.prodname_dotcom %}, em conjunto com outros recursos e produtos pagos, são movidos para o novo ciclo de cobrança na próxima data de cobrança.

## Alterar a duração do ciclo de cobrança da sua conta pessoal

{% data reusables.user_settings.access_settings %}
{% data reusables.user_settings.billing_plans %}
{% data reusables.dotcom_billing.change_plan_duration %}
{% data reusables.dotcom_billing.confirm_duration_change %}

## Alterar a duração do ciclo de cobrança da organização

{% data reusables.dotcom_billing.org-billing-perms %}

### Alterar a duração de uma assinatura por usuário

{% data reusables.organizations.billing-settings %}
{% data reusables.dotcom_billing.change_plan_duration %}
{% data reusables.dotcom_billing.confirm_duration_change %}

### Alterar a duração de um plano herdado por repositório

{% data reusables.organizations.billing-settings %}
4. Em "Billing overview" (Visão geral de cobrança), clique em **Change plan** (Alterar plano). ![Botão de alteração de plano na visão geral de cobrança](/assets/images/help/billing/billing_overview_change_plan.png)
5. No canto superior direito, clique em **Switch to monthly billing** (Alternar para cobrança mensal) ou **Switch to yearly billing** (Alternar para cobrança anual). ![Seção de informações de cobrança](/assets/images/help/billing/settings_billing_organization_plans_switch_to_yearly.png)
