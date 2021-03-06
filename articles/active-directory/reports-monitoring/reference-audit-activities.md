---
title: Referência de atividades de auditoria do Azure AD (Azure AD) | Microsoft Docs
description: Obtenha uma visão geral das atividades de auditoria que podem ser registradas em log nos seus logs de auditoria do Azure AD (Azure Active Directory).
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 4d86bcf99d5b4cf2b04980e72a2bd0a422233dc3
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622867"
---
# <a name="azure-ad-audit-activity-reference"></a>Referência das atividades de auditoria do Azure AD

Com os relatórios do Azure AD (Azure Active Directory), é possível obter as informações necessárias para determinar o desempenho do ambiente.

A arquitetura de relatório no Azure AD consiste nos seguintes componentes:

- **Relatórios de atividades** 
    - [Entradas](concept-sign-ins.md) – Fornece informações sobre o uso de aplicativos gerenciados e atividades de entrada do usuário
    - [Logs de auditoria](concept-audit-logs.md) – Permitem o rastreio de todas as alterações feitas por vários recursos no Azure AD por meio de logs. 
    
- **Relatórios de segurança** 
    - [Entradas arriscadas](concept-risky-sign-ins.md) - uma entrada arriscada é um indicador para uma tentativa de logon que pode ter sido realizada por alguém que não é o proprietário legítimo de uma conta de usuário. 
    - [Usuários sinalizados para riscos](concept-user-at-risk.md) - um usuário arriscado é um indicador de uma conta de usuário que pode ter sido comprometida. 

Este artigo lista as atividades de auditoria que podem ser registradas nos logs de auditoria.

## <a name="access-reviews"></a>Análises de acesso

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de diretórios|Adicionar unidade administrativa|
|Gerenciamento de diretórios|Adicionar membro à unidade administrativa|
|Gerenciamento de diretórios|Excluir unidade administrativa|
|Gerenciamento de diretórios|Remover membro de unidade administrativa|
|Gerenciamento de diretórios|Atualizar unidade administrativa|
|Gerenciamento de diretórios|Administração|
|Gerenciamento de Usuários|Operação do diretório|
|Gerenciamento de Usuários|Exportação|
|Gerenciamento de Usuários|Importar|
|Gerenciamento de Usuários|Outros|
|Gerenciamento de Usuários|Caução de processo|
|Gerenciamento de Usuários|Ação de regra de sincronização|
|Gerenciamento de Usuários|Adicionar aplicativo|
|Gerenciamento de Usuários|Excluir aplicativo|
|Gerenciamento de Usuários|Atualizar aplicativo|
|Gerenciamento de Usuários|Atualizar Modo de Logon Único do aplicativo|
|Gerenciamento de Usuários|Substituição de senha automática|
|Gerenciamento de Usuários|Adicionar permissões da aplicativo V2|
|Gerenciamento de Usuários|Criar aplicativo V1|
|Gerenciamento de Usuários|Criar aplicativo V2|
|Gerenciamento de Usuários|Excluir aplicativo V1|
|Gerenciamento de Usuários|Excluir aplicativo V2|
|Gerenciamento de Usuários|Excluir concessão de permissão de aplicativo V2|
|Gerenciamento de Usuários|Obter aplicativos V1 e V2|
|Gerenciamento de Usuários|Obter aplicativo V1|
|Gerenciamento de Usuários|Obter aplicativos V1|
|Gerenciamento de Usuários|Obter aplicativo V2|
|Gerenciamento de Usuários|Obter aplicativos V2|

## <a name="account-provisioning"></a>Provisionamento de conta

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Recuperar concessões de permissões de aplicativo V2|
|Gerenciamento de Aplicativos|Recuperar entidades de serviço de aplicativo V2 no locatário atual|
|Gerenciamento de Aplicativos|Atualizar aplicativo V1|
|Gerenciamento de Aplicativos|Atualizar aplicativo V2|
|Gerenciamento de Aplicativos|Atualizar concessão de permissão de aplicativo V2|
|Gerenciamento de Aplicativos|Adicionar Oauth2Permissiongrant|
|Gerenciamento de Aplicativos|Adicionar atribuição de função de aplicativo à entidade de serviço|

## <a name="application-proxy"></a>Proxy de aplicativo

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Adicionar aplicativo|
|Gerenciamento de Aplicativos|Adicionar proprietário ao aplicativo|
|Gerenciamento de Aplicativos|Adicionar proprietário à entidade de serviço|
|Gerenciamento de Aplicativos|Adicionar política à entidade de serviço|
|Gerenciamento de diretórios|Adicionar entidade de serviço|
|Gerenciamento de diretórios|Adicionar credenciais de entidade de serviço|
|Gerenciamento de diretórios|Autorizar aplicativo|
|Gerenciamento de diretórios|Excluir aplicativo|
|Gerenciamento de diretórios|Excluir aplicativo irreversivelmente|
|Gerenciamento de diretórios|Remover OAuth2PermissionGrant|
|Gerenciamento de diretórios|Remover a atribuição de função de aplicativo da entidade de serviço|
|Gerenciamento de diretórios|Remover proprietário do aplicativo|
|Recurso|Remover proprietário da entidade de serviço|
|Recurso|Remover política da entidade de serviço|
|Recurso|Remover entidade de serviço|


## <a name="automated-password-rollover"></a>Substituição de senha automática

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Remover credenciais de entidade de serviço|


## <a name="b2c"></a>B2C

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Restaurar aplicativo|
|Gerenciamento de Aplicativos|Revogar autorização|
|Gerenciamento de Aplicativos|Atualizar aplicativo|
|Gerenciamento de Aplicativos|Atualizar segredos externos|
|Gerenciamento de Aplicativos|Atualizar entidade de serviço|
|Gerenciamento de Aplicativos|Emitir um token de acesso para o aplicativo|
|Gerenciamento de Aplicativos|Emitir um código de autorização para o aplicativo|
|Gerenciamento de Aplicativos|Emitir um id_token para o aplicativo|
|Gerenciamento de Aplicativos|Validar credenciais de conta local|
|Gerenciamento de Aplicativos|Validar autenticação de usuário|
|Gerenciamento de Aplicativos|Adicionar permissões da aplicativo V2|
|Gerenciamento de Aplicativos|Adicionar uma chave baseada em segredo ASCII a um contêiner de chave CPIM|
|Gerenciamento de Aplicativos|Adicionar uma chave a um contêiner de chave CPIM|
|Gerenciamento de Aplicativos|AdminPolicyDatas-SetResources|
|Gerenciamento de Aplicativos|AdminUserJourneys-GetResources|
|Gerenciamento de Aplicativos|AdminUserJourneys-RemoveResources|
|Autenticação|AdminUserJourneys-SetResources|
|Autenticação|Criar IdentityProvider|
|Autenticação|Criar aplicativo V1|
|Autenticação|Criar aplicativo V2|
|Autenticação|Criar um domínio personalizado no locatário|
|Autorização|Criar um novo AdminUserJourney|
|Autorização|Criar json de recurso localizado|
|Autorização|Criar novo IDP personalizado|
|Autorização|Criar novo IDP|
|Autorização|Criar ou atualizar um recurso de diretório do B2C|
|Autorização|Criar política|
|Autorização|Criar política trustFramework|
|Autorização|Criar política trustFramework com prefixo configurável|
|Autorização|Criar atributo de usuário|
|Autorização|CreateTrustFrameworkPolicy|
|Autorização|Cria ou atualiza um novo AdminUserJourney|
|Autorização|Excluir IDP|
|Autorização|Excluir IdentityProvider|
|Autorização|Excluir aplicativo V1|
|Autorização|Excluir aplicativo V2|
|Autorização|Excluir concessão de permissão de aplicativo V2|
|Autorização|Excluir um recurso do diretório do B2C|
|Autorização|Excluir um contêiner de chave CPIM|
|Autorização|Excluir política trustFramework|
|Autorização|Excluir atributo de usuário|
|Autorização|Habilitar recurso do B2C|
|Autorização|Obter os recursos de diretório do B2C em uma assinatura|
|Autorização|Obter IDP personalizado|
|Autorização|Obter IDP|
|Autorização|Obter aplicativos V1 e V2|
|Autorização|Obter aplicativo V1|
|Autorização|Obter aplicativos V1|
|Autorização|Obter aplicativo V2|
|Autorização|Obter aplicativos V2|
|Autorização|Obter um recurso de diretório do B2C|
|Autorização|Obter uma lista de domínios personalizados no locatário|
|Autorização|Obter um Percurso do Usuário|
|Autorização|Obter declarações do aplicativo permitidas para o Percurso do Usuário|
|Autorização|Obter declarações autodeclaradas permitidas para o Percurso do Usuário|
|Autorização|Obter declarações autodeclaradas permitidas de política|
|Autorização|Obter lista de declarações de saída disponíveis|
|Autorização|Obter definições de conteúdo para o Percurso do Usuário|
|Autorização|Obter IDPs para um fluxo específico do administrador|
|Autorização|Obter metadados de chave ativa do contêiner de chave em JWK|
|Autorização|Obter lista de todos os fluxos de administrador|
|Autorização|Obter lista de marcas de todos os fluxos de administrador para todos os usuários|
|Autorização|Obter lista de locatários de um usuário|
|Autorização|Obter declarações autodeclaradas das contas locais|
|Autorização|Obter json de recurso localizado|
|Autorização|Obter operações do provedor de recursos Microsoft.AzureActiveDirectory|
|Autorização|Obter políticas|
|Autorização|Obter política|
|Autorização|Obter propriedades de recurso de um locatário|
|Autorização|Obter lista de IDP com suporte|
|Autorização|Obter lista de IDP com suporte do Percurso do Usuário|
|Autorização|Obter informações do locatário|
|Autorização|Obter recursos permitidos para o locatário|
|Autorização|Obter lista de IDPs personalizadas definida pelo locatário|
|Autorização|Obter lista de IDPs definida pelo locatário|
|Autorização|Obter lista de IDPs locais definida pelo locatário|
|Autorização|Obter detalhes do locatário de um usuário para a criação de recursos|
|Autorização|Obter lista de locatários|
|Autorização|Obter tenantDomains|
|Autorização|Obter a cultura com suporte padrão para CPIM|
|Autorização|Obter os detalhes de um fluxo de administrador|
|Autorização|Obter a lista de UserJourneys para o locatário|
|Autorização|Obter o conjunto de culturas com suporte disponíveis para CPIM|
|Autorização|Obter política trustFramework|
|Autorização|Obter política trustFramework como xml|
|Autorização|Editar atributo de usuário|
|Autorização|Obter atributos de usuário|
|Autorização|Obter lista de Percurso do Usuário|
|Autorização|GetIEFPolicies|
|Autorização|GetIdentityProviders|
|Autorização|GetTrustFrameworkPolicy|
|Autorização|Obtém um contêiner de chave CPIM em formato jwk|
|Autorização|Obtém uma lista de contêineres de chave no locatário|
|Autorização|Obtém o tipo de locatário|
|Autorização|MigrateTenantMetadata|
|Autorização|Fazer patch de IdentityProvider|
|Autorização|PutTrustFrameworkPolicy|
|Autorização|PutTrustFrameworkpolicy|
|Autorização|Remover um Percurso do Usuário|
|Autorização|Restaurar um backup de contêiner de chave CPIM|
|Autorização|Recuperar concessões de permissões de aplicativo V2|
|Autorização|Recuperar entidades de serviço de aplicativo V2 no locatário atual|
|Autorização|Atualizar IDP personalizado|
|Autorização|Atualizar IDP|
|Autorização|Atualizar IDP local|
|Autorização|Atualizar aplicativo V1|
|Autorização|Atualizar aplicativo V2|
|Autorização|Atualizar concessão de permissão de aplicativo V2|
|Autorização|Atualizar política|
|Autorização|Atualizar atributo de usuário|
|Autorização|Carregar uma chave CPIM criptografada|
|Autorização|Autorização do usuário: a API está desabilitada para o conjunto de recursos do locatário|
|Autorização|Autorização do usuário: acesso concedido ao usuário como ‘Administrador do locatário’|
|Autorização|Autorização do usuário: direitos de acesso concedidos ao usuário como 'Usuários autenticados'|
|Autorização|Verificar se o recurso B2C está habilitado|
|Autorização|Verificar se o recurso está habilitado|
|Autorização|Criar programa|
|Autorização|Excluir programa|
|Autorização|Vincular controle do programa|
|Autorização|Integrar com as Revisões de Acesso do Azure AD|
|Autorização|Desvincular controle do programa|
|Autorização|Atualizar programa|
|Autorização|Desabilitar SSO da Área de Trabalho|
|Autorização|Desabilitar SSO da Área de Trabalho para um domínio específico|
|Autorização|Desabilitar proxy de aplicativo|
|Autorização|Desabilitar autenticação de passagem|
|Autorização|Habilitar SSO da Área de Trabalho|
|Gerenciamento de diretórios|Habilitar SSO da Área de Trabalho para um domínio específico|
|Gerenciamento de diretórios|Habilitar proxy de aplicativo|
|Gerenciamento de diretórios|Habilitar autenticação de passagem|
|Gerenciamento de diretórios|Criar um domínio personalizado no locatário|
|Gerenciamento de diretórios|Habilitar recurso do B2C|
|Gerenciamento de diretórios|Obter uma lista de domínios personalizados no locatário|
|Gerenciamento de diretórios|Obter propriedades de recurso de um locatário|
|Gerenciamento de diretórios|Obter informações do locatário|
|Gerenciamento de diretórios|Obter recursos permitidos para o locatário|
|Gerenciamento de diretórios|Obter tenantDomains|
|Chave|Obtém o tipo de locatário|
|Chave|Verificar se o recurso B2C está habilitado|
|Chave|Verificar se o recurso está habilitado|
|Chave|Adicionar um parceiro à empresa|
|Chave|Adicionar domínio não verificado|
|Chave|Adicionar domínio verificado|
|Chave|Criar empresa|
|Chave|Criar configurações da empresa|
|Chave|Excluir configurações da empresa|
|Chave|Rebaixar parceiro|
|Chave|Diretório excluído|
|Outros|Diretório excluído permanentemente|
|Outros|Diretório agendado para exclusão|
|Recurso|Promover empresa à condição de parceira|
|Recurso|Limpar propriedades de gerenciamento de direitos|
|Recurso|Remover parceiro da empresa|
|Recurso|Remover domínio não verificado|
|Recurso|Remover domínio verificado|
|Recurso|Definir informações da empresa|
|Recurso|Definir recurso DirSync|
|Recurso|Definir sinalizador DirSyncEnabled|
|Recurso|Definir parceria|
|Recurso|Definir limite de exclusão acidental|
|Recurso|Definir local de dados permitido da empresa|
|Recurso|Definir recurso multinacional de empresa como habilitado|
|Recurso|Definir recurso de diretório no locatário|
|Recurso|Definir a autenticação de domínio|
|Recurso|Definir configurações de federação no domínio|
|Recurso|Definir política de senha|
|Recurso|Definir propriedades de gerenciamento de direitos|
|Recurso|Atualizar empresa|
|Recurso|Atualizar configurações da empresa|
|Recurso|Domínio de atualização|
|Recurso|Verificar domínio|
|Recurso|Verificar domínio por email|
|Recurso|Integração|
|Recurso|Atualizar configurações de alerta|
|Recurso|Atualizar configurações de resumo semanal|
|Recurso|Desabilitar write-back de senha para o diretório|
|Recurso|Habilitar write-back de senha para o diretório|
|Recurso|Adicionar atribuição de função de aplicativo ao grupo|
|Recurso|Adicionar grupo|
|Recurso|Adicionar membro ao grupo|
|Recurso|Adicionar proprietário ao grupo|
|Recurso|Criar configurações de grupo|
|Recurso|Excluir grupo|
|Recurso|Excluir configurações de grupo|
|Recurso|Concluir a aplicação de licença com base em grupo aos usuários|
|Recurso|Excluir grupo irreversivelmente|
|Recurso|Remover atribuição de função de aplicativo do grupo|
|Recurso|Remover membro do grupo|
|Recurso|Remover proprietário do grupo|
|Recurso|Restaurar grupo|
|Recurso|Definir licença de grupo|
|Recurso|Definir grupo a ser gerenciado pelo usuário|
|Recurso|Iniciar a aplicação de licença com base em grupo aos usuários|
|Recurso|Disparar recálculo de licença do grupo|
|Recurso|Atualizar grupo|
|Recurso|Atualizar configurações de grupo|
|Recurso|Adicionar membro|
|Recurso|Criar Grupo|
|Recurso|Excluir grupo|
|Recurso|Remover membro|
|Recurso|Atualizar grupo|
|Recurso|Aprovar uma solicitação pendente para ingressar em um grupo|
|Recurso|Cancelar uma solicitação pendente para ingressar em um grupo|
|Recurso|Criar política de gerenciamento de ciclo de vida|
|Recurso|Excluir uma solicitação pendente para ingressar em um grupo|
|Recurso|Rejeitar uma solicitação pendente para ingressar em um grupo|
|Recurso|Renovar grupo|
|Recurso|Solicitar ingresso em um grupo|
|Recurso|Definir propriedades de grupo dinâmico|
|Recurso|Atualizar política de gerenciamento de ciclo de vida|
|Recurso|Adicionar uma chave baseada em segredo ASCII a um contêiner de chave CPIM|
|Recurso|Adicionar uma chave a um contêiner de chave CPIM|
|Recurso|Excluir um contêiner de chave CPIM|
|Recurso|Excluir contêiner de chave|
|Recurso|Obter metadados de chave ativa do contêiner de chave em JWK|
|Recurso|Obter metadados do contêiner de chave|
|Recurso|Obtém um contêiner de chave CPIM em formato jwk|
|Recurso|Obtém uma lista de contêineres de chave no locatário|
|Recurso|Restaurar um backup de contêiner de chave CPIM|
|Recurso|Salvar contêiner de chave|
|Recurso|Carregar uma chave CPIM criptografada|
|Recurso|Emitir um código de autorização para o aplicativo|
|Recurso|Emitir um id_token para o aplicativo|


## <a name="core-directory"></a>Diretório principal

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de unidades administrativas|Baixar um único tipo de evento de risco|
|Gerenciamento de unidades administrativas|Baixar administradores e status de aceitação do resumo semanal|
|Gerenciamento de unidades administrativas|Baixar todos os tipos de evento de risco|
|Gerenciamento de unidades administrativas|Baixar eventos de risco de usuário gratuitos|
|Gerenciamento de unidades administrativas|Baixar usuários sinalizados por risco|
|Gerenciamento de Aplicativos|Convites do Lote processados|
|Gerenciamento de Aplicativos|Convites do Lote carregados|
|Gerenciamento de Aplicativos|Adicionar proprietário à política|
|Gerenciamento de Aplicativos|Adicionar política|
|Gerenciamento de Aplicativos|Excluir política|
|Gerenciamento de Aplicativos|Remover credenciais de política|
|Gerenciamento de Aplicativos|Atualizar política|
|Gerenciamento de Aplicativos|Definir política de registro MFA|
|Gerenciamento de Aplicativos|Definir política de risco de entrada|
|Gerenciamento de Aplicativos|Definir política de risco do usuário|
|Gerenciamento de Aplicativos|Aceitar os Termos de Uso|
|Gerenciamento de Aplicativos|Criar Termos de Uso|
|Gerenciamento de Aplicativos|Recusar Termos de Uso|
|Gerenciamento de Aplicativos|Excluir Termos de Uso|
|Gerenciamento de Aplicativos|Editar Termos de Uso|
|Gerenciamento de Aplicativos|Publicar Termos de Uso|
|Gerenciamento de Aplicativos|Cancelar a publicação dos Termos de Uso|
|Gerenciamento de Aplicativos|Adicionar certificado SSL do aplicativo|
|Gerenciamento de Aplicativos|Excluir associação SSL|
|Gerenciamento de Aplicativos|Registrar conector|
|Gerenciamento de Aplicativos|AdminPolicyDatas-RemoveResources|
|Gerenciamento de Aplicativos|AdminPolicyDatas-SetResources|
|Gerenciamento de Aplicativos|AdminUserJourneys-GetResources|
|Gerenciamento de diretórios|AdminUserJourneys-RemoveResources|
|Gerenciamento de diretórios|AdminUserJourneys-SetResources|
|Gerenciamento de diretórios|Criar IdentityProvider|
|Gerenciamento de diretórios|Criar um novo AdminUserJourney|
|Gerenciamento de diretórios|Criar json de recurso localizado|
|Gerenciamento de diretórios|Criar novo IDP personalizado|
|Gerenciamento de diretórios|Criar novo IDP|
|Gerenciamento de diretórios|Criar ou atualizar um recurso de diretório do B2C|
|Gerenciamento de diretórios|Criar política|
|Gerenciamento de diretórios|Criar política trustFramework|
|Gerenciamento de diretórios|Criar política trustFramework com prefixo configurável|
|Gerenciamento de diretórios|Criar atributo de usuário|
|Gerenciamento de diretórios|CreateTrustFrameworkPolicy|
|Gerenciamento de diretórios|Excluir IDP|
|Gerenciamento de diretórios|Excluir IdentityProvider|
|Gerenciamento de diretórios|Excluir um recurso do diretório do B2C|
|Gerenciamento de diretórios|Excluir política trustFramework|
|Gerenciamento de diretórios|Excluir atributo de usuário|
|Gerenciamento de diretórios|Obter recursos de diretório do B2C em um grupo de recursos|
|Gerenciamento de diretórios|Obter os recursos de diretório do B2C em uma assinatura|
|Gerenciamento de diretórios|Obter IDP personalizado|
|Gerenciamento de diretórios|Obter IDP|
|Gerenciamento de diretórios|Obter um recurso de diretório do B2C|
|Gerenciamento de diretórios|Obter um Percurso do Usuário|
|Gerenciamento de diretórios|Obter declarações do aplicativo permitidas para o Percurso do Usuário|
|Gerenciamento de diretórios|Obter declarações autodeclaradas permitidas para o Percurso do Usuário|
|Gerenciamento de diretórios|Obter declarações autodeclaradas permitidas de política|
|Gerenciamento de diretórios|Obter lista de declarações de saída disponíveis|
|Gerenciamento de diretórios|Obter definições de conteúdo para o Percurso do Usuário|
|Gerenciamento de diretórios|Obter IDPs para um fluxo específico do administrador|
|Gerenciamento de diretórios|Obter lista de todos os fluxos de administrador|
|Gerenciamento de diretórios|Obter lista de marcas de todos os fluxos de administrador para todos os usuários|
|Gerenciamento de grupos|Obter lista de locatários de um usuário|
|Gerenciamento de grupos|Obter declarações autodeclaradas das contas locais|
|Gerenciamento de grupos|Obter json de recurso localizado|
|Gerenciamento de grupos|Obter operações do provedor de recursos Microsoft.AzureActiveDirectory|
|Gerenciamento de grupos|Obter políticas|
|Gerenciamento de grupos|Obter política|
|Gerenciamento de grupos|Obter lista de IDP com suporte|
|Gerenciamento de grupos|Obter lista de IDP com suporte do Percurso do Usuário|
|Gerenciamento de grupos|Obter lista de IDPs personalizadas definida pelo locatário|
|Gerenciamento de grupos|Obter lista de IDPs definida pelo locatário|
|Gerenciamento de grupos|Obter lista de IDPs locais definida pelo locatário|
|Gerenciamento de grupos|Obter detalhes do locatário de um usuário para a criação de recursos|
|Gerenciamento de grupos|Obter a cultura com suporte padrão para CPIM|
|Gerenciamento de grupos|Obter os detalhes de um fluxo de administrador|
|Gerenciamento de grupos|Obter a lista de UserJourneys para o locatário|
|Gerenciamento de grupos|Obter o conjunto de culturas com suporte disponíveis para CPIM|
|Gerenciamento de grupos|Obter política trustFramework|
|Gerenciamento de grupos|Obter política trustFramework como xml|
|Gerenciamento de grupos|Editar atributo de usuário|
|Gerenciamento de política|Obter atributos de usuário|
|Gerenciamento de política|Obter lista de Percurso do Usuário|
|Gerenciamento de política|GetIEFPolicies|
|Gerenciamento de política|GetIdentityProviders|
|Gerenciamento de política|GetTrustFrameworkPolicy|
|Recurso|MigrateTenantMetadata|
|Recurso|Mover recursos|
|Recurso|Fazer patch de IdentityProvider|
|Recurso|PutTrustFrameworkPolicy|
|Recurso|PutTrustFrameworkpolicy|
|Recurso|Remover um Percurso do Usuário|
|Recurso|Atualizar IDP personalizado|
|Recurso|Atualizar IDP|
|Recurso|Atualizar IDP local|
|Recurso|Exibir um recurso de diretório do B2C|
|Recurso|Atualizar política|
|Recurso|Atualizar status da assinatura|
|Gerenciamento de funções|Atualizar atributo de usuário|
|Gerenciamento de funções|Validar movimentação de recursos|
|Gerenciamento de funções|Adicionar dispositivo|
|Gerenciamento de funções|Adicionar configuração do dispositivo|
|Gerenciamento de funções|Adicionar proprietário registrado ao dispositivo|
|Gerenciamento de funções|Adicionar usuários registrados ao dispositivo|
|Gerenciamento de funções|Excluir um dispositivo|
|Gerenciamento de funções|Excluir configuração de dispositivo|
|Gerenciamento de funções|O dispositivo não é mais compatível|
|Gerenciamento de funções|O dispositivo não é mais gerenciado|
|Gerenciamento de Usuários|Remover proprietário registrado do dispositivo|
|Gerenciamento de Usuários|Remover usuários registrados do dispositivo|
|Gerenciamento de Usuários|Atualizar dispositivo|
|Gerenciamento de Usuários|Atualizar configuração do dispositivo|
|Gerenciamento de Usuários|Adicionar membro qualificado à função|
|Gerenciamento de Usuários|Adicionar membro à função|
|Gerenciamento de Usuários|Adicionar atribuição de função à definição de função|
|Gerenciamento de Usuários|Adicionar função originada no modelo|
|Gerenciamento de Usuários|Adicionar membro no escopo à função|
|Gerenciamento de Usuários|Remover membro qualificado da função|
|Gerenciamento de Usuários|Remover membro da função|
|Gerenciamento de Usuários|Remover atribuição de função da definição de função|
|Gerenciamento de Usuários|Remover membro no escopo da função|
|Gerenciamento de Usuários|Atualizar função|
|Gerenciamento de Usuários|AccessReview_Review|
|Gerenciamento de Usuários|AccessReview_Update|
|Gerenciamento de Usuários|ActivationAborted|
|Gerenciamento de Usuários|ActivationApproved|
|Gerenciamento de Usuários|ActivationCanceled|
|Gerenciamento de Usuários|ActivationRequested|
|Gerenciamento de Usuários|Added|
|Gerenciamento de Usuários|Assign|


## <a name="identity-protection"></a>Identity Protection

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de diretórios|Elevate|
|Gerenciamento de diretórios|Removed|
|Gerenciamento de diretórios|Alterações na configuração de função|
|Outros|ScanAlertsNow|
|Outros|Inscrever-se|
|Outros|Unelevate|
|Outros|UpdateAlertSettings|
|Outros|UpdateCurrentState|
|Gerenciamento de política|Revisão de acesso encerrada|
|Gerenciamento de política|Adicionar aprovador para a aprovação de solicitação|
|Gerenciamento de política|Adicionar revisor para a revisão de acesso|
|Gerenciamento de Usuários|Aplicar revisão de acesso|
|Gerenciamento de Usuários|Criar a revisão de acesso|


## <a name="invited-users"></a>Usuários convidados

|Auditar categoria|Atividade|
|---|---|
|Outros|Criar solicitação de aprovação|
|Outros|Excluir revisão de acesso|
|Gerenciamento de Usuários|Remover revisor da revisão de acesso|
|Gerenciamento de Usuários|Solicitar aplicação dos resultados da análise|
|Gerenciamento de Usuários|Solicitar interrupção da análise|
|Gerenciamento de Usuários|Revisar atribuição do aplicativo|
|Gerenciamento de Usuários|Revisar associação ao grupo|
|Gerenciamento de Usuários|Examinar associação de função RBAC|


## <a name="microsoft-identity-manager-mim"></a>MIM (Microsoft Identity Manager)

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de grupos|Revisar solicitação de aprovação de solicitação|
|Gerenciamento de grupos|Atualizar revisão de acesso|
|Gerenciamento de grupos|Atualizar configurações de notificação de email da revisão de acesso|
|Gerenciamento de grupos|Atualizar configuração da contagem de recorrência da revisão de acesso|
|Gerenciamento de grupos|Atualizar configuração da duração de recorrência da revisão de acesso em dias|
|Gerenciamento de Usuários|Atualizar configuração de tipo e recorrência da revisão de acesso|
|Gerenciamento de Usuários|Atualizar configuração de tipo de recorrência da revisão de acesso|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de funções|Atualizar configurações de lembrete de revisão de acesso|
|Gerenciamento de funções|Atualizar aprovação de solicitação|
|Gerenciamento de funções|Adicionar concessão de atribuição de função de aplicativo ao usuário|
|Gerenciamento de funções|Adicionar usuário|
|Gerenciamento de funções|Adicionar detalhes de aplicativo de telefone de autenticação forte do usuário|
|Gerenciamento de funções|Alterar licença de usuário|
|Gerenciamento de funções|Alterar senha de usuário|
|Gerenciamento de funções|Converter usuário federado em gerenciado|
|Gerenciamento de funções|Criar senha de aplicativo para o usuário|
|Gerenciamento de funções|Excluir senha de aplicativo do usuário|
|Gerenciamento de funções|Excluir usuário|
|Gerenciamento de funções|Desabilitar conta|
|Gerenciamento de funções|Habilitar autenticação forte|
|Gerenciamento de funções|Excluir usuário irreversivelmente|
|Gerenciamento de funções|Remover atribuição de função de aplicativo do usuário|
|Gerenciamento de funções|Remover detalhes de aplicativo de telefone de autenticação forte do usuário|



## <a name="self-service-group-management"></a>Gerenciamento de grupo de autoatendimento

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de grupos|Redefinir senha de usuário|
|Gerenciamento de grupos|Restaurar usuário|
|Gerenciamento de grupos|Definir alteração forçada de senha de usuário|
|Gerenciamento de grupos|Definir gerenciador de usuários|
|Gerenciamento de grupos|Definir metadados de token OAuth de usuários como habilitados|
|Gerenciamento de grupos|Atualizar carimbo de data/hora StsRefreshTokenValidFrom|
|Gerenciamento de grupos|Atualizar segredos externos|
|Gerenciamento de grupos|Atualizar usuário|
|Gerenciamento de grupos|O administrador gera uma senha temporária|


## <a name="self-service-password-management"></a>Gerenciamento de senhas de autoatendimento

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de diretórios|Os administradores exigem que o usuário redefina a senha|
|Gerenciamento de diretórios|Atribuir usuário externo ao aplicativo|
|Gerenciamento de Usuários|Email não enviado; o usuário cancelou a assinatura|
|Gerenciamento de Usuários|Convidar usuário externo|
|Gerenciamento de Usuários|Resgatar convite para usuário externo|
|Gerenciamento de Usuários|Criação de locatário viral|
|Gerenciamento de Usuários|Criação de usuário viral|
|Gerenciamento de Usuários|Registro de senha do usuário|
|Gerenciamento de Usuários|Redefinição de senha do usuário|
|Gerenciamento de Usuários|Impedido de executar a redefinição de senha de autoatendimento|


## <a name="terms-of-use"></a>Termos de uso

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de política|Alterar senha (autoatendimento)|
|Gerenciamento de política|Redefinir senha (pelo administrador)|
|Gerenciamento de política|Redefinir senha (autoatendimento)|
|Gerenciamento de política|Progresso da atividade de fluxo de redefinição de senha de autoatendimento|
|Gerenciamento de política|Progresso da atividade de fluxo de redefinição de senha de autoatendimento|
|Gerenciamento de política|Desbloquear conta de usuário (autoatendimento)|
|Gerenciamento de política|Usuário registrado para redefinição de senha de autoatendimento|


## <a name="next-steps"></a>Próximas etapas

- [Visão geral dos relatórios do Azure AD](overview-reports.md).
- [Relatório de logs de auditoria](concept-audit-logs.md). 
- [Acesso programático aos relatórios do Azure AD](concept-reporting-api.md)
