# Documentação Técnica: Análise do Projeto e Estudo de Viabilidade de Implantação — SISMUMA

---

## Sumário
1. [Visão Geral e Arquitetura do Sistema](#1-visão-geral-e-arquitetura-do-sistema)
2. [Análise Estrutural e Módulos Core](#2-análise-estrutural-e-módulos-core)
3. [Conclusão e Diagnóstico do Projeto](#3-conclusão-e-diagnóstico-do-projeto)
4. [Estudo de Viabilidade de Implantação (GitHub Pages + Supabase)](#4-estudo-de-viabilidade-de-implantação-github-pages--supabase)
   - [4.1. Arquitetura Proposta](#41-arquitetura-proposta)
   - [4.2. Análise de Viabilidade Técnica e Geográfica](#42-análise-de-viabilidade-técnica-e-geográfica)
   - [4.3. Análise Econômica e Operacional](#43-análise-econômica-e-operacional)
5. [Plano de Implantação Passo a Passo](#5-plano-de-implantação-passo-a-passo)
6. [Segurança e Compliance Legal](#6-segurança-e-compliance-legal)

---

## 1. Visão Geral e Arquitetura do Sistema

O **SISMUMA** (*Sistema de Licenciamento e Gestão Ambiental Municipal*) é uma solução voltada para a modernização, automação e fiscalização dos processos de competência dos órgãos ambientais municipais.

### 1.1. Propósito do Sistema
* **Desburocratização do Licenciamento Ambiental Municipal (LAM):** Controle do fluxo de emissão de Licenças Prévias (LP), Licenças de Instalação (LI), Licenças de Operação (LO) e Autorizações Ambientais (AA) para atividades e empreendimentos de impacto local.
* **Fiscalização e Monitoramento:** Gestão centralizada de denúncias ambientais, emissão de Autos de Infração, Termos de Ajustamento de Conduta (TAC) e acompanhamento de vistorias técnicas de campo.
* **Governança Digital e Transparência:** Rastreabilidade integral das etapas de análise, reduzindo o tempo de tramitação (*SLA*) de processos administrativos.

---

## 2. Análise Estrutural e Módulos Core

O sistema contempla uma divisão modular desenhada para atender aos múltiplos atores envolvidos na gestão pública municipal:

```
                  +-----------------------------------+
                  |          SISMUMA CORE             |
                  +-----------------------------------+
                                    |
     +------------------+-----------+-----------+------------------+
     |                  |                       |                  |
+----+-----+      +-----+------+          +-----+-----+      +-----+-----+
|  Módulo  |      |  Módulo de |          | Módulo de |      | Módulo SIG|
|  RBAC    |      |Licenciamento|         |Fiscalização|      |(Geográfico)|
+----------+      +------------+          +-----------+      +-----------+
```

### 2.1. Descrição dos Módulos

1. **Módulo de Controle de Acesso Baseado em Papéis (RBAC):**
   * **Cidadão / Empreendedor:** Requerimento de licenças, envio de documentações e acompanhamento do status do processo.
   * **Atendimento / Protocolo:** Triagem inicial, conferência documental e abertura formal do processo.
   * **Analista Ambiental:** Emissão de pareceres técnicos, solicitação de exigências complementares e parecer final.
   * **Fiscal Ambiental:** Emissão de notificações, laudos de vistoria e autos de infração.
   * **Gestor / Secretário:** Homologação, assinatura digital de certidões/licenças e visualização de dashboards gerenciais.

2. **Fluxo de Licenciamento Digital:**
   * Enquadramento automatizado da atividade conforme o potencial poluidor e porte da empresa (alinhado a Resoluções CONAMA/CONSEMA).
   * Checklists dinâmicos para exigência de estudos ambientais (PIR, RCA, PCA, PRADA).

3. **Georreferenciamento e Análise Espacial (SIG):**
   * Processamento e cruzamento de coordenadas geográficas (UTM e Lat/Long) e arquivo vetorial (Shapefile/KML/GeoJSON) da gleba com camadas territoriais de restrição ambiental:
     * Áreas de Preservação Permanente (APPs)
     * Unidades de Conservação (UCs)
     * Zonas de Amortecimento e Hidrografia

4. **Gestão de Documentos e Certidões:**
   * Geração automatizada de documentos oficiais formatados em PDF com validação e código QR para verificação de autenticidade.

---

## 3. Conclusão e Diagnóstico do Projeto

O **SISMUMA** consolida-se como um projeto de **alto impacto social e tecnológico**. A transição da gestão ambiental baseada em processos físicos de papel para uma plataforma digital unificada proporciona:

1. **Eficiência Operacional:** Redução expressiva no tempo médio para concessão ou indeferimento de licenças ambientais.
2. **Segurança Jurídica:** Registro imutável de todas as etapas do parecer técnico, garantindo auditoria e conformidade com a **Lei Complementar nº 140/2011**.
3. **Precisão Territorial:** Redução substancial de falhas humanas na verificação de sobreposição de áreas de preservação ambiental por meio do uso de ferramentas geográficas.

O projeto apresenta uma base conceitual e prática alinhada às melhores tendências de *GovTechs*, apresentando alta escalabilidade e potencial de implantação em municípios de diversos portes.

---

## 4. Estudo de Viabilidade de Implantação (GitHub Pages + Supabase)

### 4.1. Arquitetura Proposta

```
+-------------------------------------------------------------------+
|                           CLIENTE / BROWSER                        |
|                                                                   |
|   Single Page Application (React / Vite + Tailwind / Leaflet)     |
+-------------------------------------------------------------------+
                                  |
                                  | HTTPS / API Call
                                  v
+-------------------------------------------------------------------+
|                        GITHUB PAGES (HOSTING)                     |
|  - Distribuição estática via CDN Global                          |
|  - Certificado SSL automático                                     |
|  - CI/CD nativo via GitHub Actions                                |
+-------------------------------------------------------------------+
                                  |
                                  | @supabase/supabase-js (JWT)
                                  v
+-------------------------------------------------------------------+
|                         SUPABASE BACKEND                          |
|  +-------------------------------------------------------------+  |
|  | PostgreSQL Database + PostGIS (Geospatial extension)        |  |
|  +-------------------------------------------------------------+  |
|  | Auth (JWT, OAuth, RBAC via Row Level Security - RLS)        |  |
|  +-------------------------------------------------------------+  |
|  | Storage (Documentos, Anexos em PDF, Imagens de Vistoria)     |  |
|  +-------------------------------------------------------------+  |
|  | Auto-Generated REST APIs & Realtime Subscriptions           |  |
+-------------------------------------------------------------------+
```

---

### 4.2. Análise de Viabilidade Técnica e Geográfica

| Componente | Tecnologia Sugerida | Papel no SISMUMA | Viabilidade |
| :--- | :--- | :--- | :--- |
| **Frontend** | React + Vite + Tailwind CSS | Interface reativa para os usuários do sistema. | **100% Viável** |
| **Hospedagem Front** | GitHub Pages | Distribuição estática da SPA de forma global e resiliente. | **100% Viável** |
| **Banco de Dados** | PostgreSQL + PostGIS (Supabase) | Armazenamento relacional de processos e processamento espacial das áreas. | **100% Viável** |
| **Autenticação** | Supabase Auth | Gerenciamento de login, recuperação de senha e papéis (RBAC). | **100% Viável** |
| **Segurança de Dados**| Row Level Security (RLS) | Controle fino de acesso diretamente nas tabelas SQL. | **100% Viável** |
| **Armazenamento** | Supabase Storage | Upload e download seguro de arquivos PDF e laudos com URLs assinadas. | **100% Viável** |

#### Detalhe Técnico: O uso do PostGIS no Supabase
O Supabase oferece suporte nativo à extensão `PostGIS`. Isso permite executar queries espaciais direto do banco de dados, como por exemplo:

```sql
-- Exemplo de verificação de sobreposição de um imóvel com APP
SELECT e.id, e.nome_empreendimento
FROM empreendimentos e, areas_preservacao app
WHERE ST_Intersects(e.geometria, app.geometria);
```

Isso elimina a necessidade de construir um servidor backend complexo dedicado exclusivamente ao processamento GIS.

---

### 4.3. Análise Econômica e Operacional

#### Comparativo de Custos Mensais (Estimativa para Município de Porte Médio)

| Recurso | Arquitetura Tradicional (VPS + Servidor Dedicado) | Arquitetura Proposta (GitHub Pages + Supabase Free/Pro) |
| :--- | :--- | :--- |
| **Hospedagem Web** | R\$ 150,00 - R\$ 300,00 / mês | **R\$ 0,00** (GitHub Pages) |
| **Banco de Dados / API** | R\$ 200,00 - R\$ 500,00 / mês | **R\$ 0,00** (Free Tier) a **~\$25,00/mês** (Pro Tier) |
| **Manutenção / DevOps** | Necessidade de SysAdmin para patches de SO e Nginx | **R\$ 0,00** (Serviço Gerenciado/Serverless) |
| **Certificados SSL** | Configuração via Let's Encrypt manual/certbot | **Incluso Nativamente** |
| **Custo Total Estimado** | **R\$ 350,00 a R\$ 800,00 / mês** | **R\$ 0,00 a ~R\$ 150,00 / mês** |

---

## 5. Plano de Implantação Passo a Passo

### Passo 1: Provisionamento da Infraestrutura no Supabase
1. Criar um novo projeto no console do Supabase.
2. Habilitar a extensão **PostGIS**:
   ```sql
   CREATE EXTENSION IF NOT EXISTS postgis;
   ```
3. Executar o script DDL de criação das tabelas centrais (`perfis`, `processos`, `requerimentos`, `licencas`, `fiscalizacao`, `geometrias_imoveis`).
4. Configurar os Buckets no **Supabase Storage**:
   * `documentos-processos` (Acesso restrito)
   * `licencas-emitidas` (Acesso público para leitura)

### Passo 2: Definição das Políticas de Segurança (RLS)
Exemplo de política RLS para garantir que o cidadão visualize apenas seus próprios processos:

```sql
-- Habilitar RLS na tabela
ALTER TABLE processos ENABLE ROW LEVEL SECURITY;

-- Política de leitura
CREATE POLICY "Usuário lê apenas seus próprios processos" 
ON processos 
FOR SELECT 
USING (auth.uid() = requerente_id OR auth.jwt() ->> 'role' IN ('analista', 'fiscal', 'gestor'));
```

### Passo 3: Ajustes e Integração do Frontend
1. Instalar o cliente do Supabase no projeto React:
   ```bash
   npm install @supabase/supabase-js
   ```
2. Inicializar o cliente em um serviço centralizado:
   ```javascript
   import { createClient } from '@supabase/supabase-js';

   const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
   const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

   export const supabase = createClient(supabaseUrl, supabaseAnonKey);
   ```

### Passo 4: Automação de CI/CD para GitHub Pages
Criar o arquivo `.github/workflows/deploy.yml` no repositório:

```yaml
name: Deploy SISMUMA Frontend to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm ci

      - name: Build Application
        env:
          VITE_SUPABASE_URL: ${{ secrets.VITE_SUPABASE_URL }}
          VITE_SUPABASE_ANON_KEY: ${{ secrets.VITE_SUPABASE_ANON_KEY }}
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: dist
          branch: gh-pages
```

---

## 6. Segurança e Compliance Legal

* **Lei Geral de Proteção de Dados (LGPD - Lei nº 13.709/2018):** Os dados pessoais de cidadãos e representantes legais armazenados no banco de dados devem possuir criptografia em repouso e em trânsito (HTTPS/TLS 1.3).
* **Autenticação Segura:** As credenciais e dados sensíveis de acesso são trafegados via HTTPS utilizando padrões JWT (JSON Web Tokens) mantidos pela infraestrutura de autenticação do Supabase.
* **Validação de Documentos Oficiais:** As licenças geradas contêm hash de validação e código QR apontando para o endpoint público de checagem no SISMUMA, garantindo a autenticidade perante terceiros e órgãos fiscalizadores federais/estaduais.

---
*Documento gerado para compor a documentação técnica oficial de implantação do projeto SISMUMA.*
