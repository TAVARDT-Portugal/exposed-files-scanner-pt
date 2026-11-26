<p align="center">
  <a href="https://ag.tavardt.com/pt/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# Scanner de Ficheiros e Credenciais Expostas

[![Verificador de Ficheiros Expostos](https://img.shields.io/badge/TAVARDT-Superficie%20de%20Ataque-5b3cf5.svg)](https://tools.tavardt.com/pt/verificador-ficheiros-expostos/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Ferramenta de varrimento automatizado de superfície de ataque para identificação de ficheiros de ambiente expostos, cópias de segurança SQL, metadados git e credenciais de plataformas cloud.

O motor analisa perímetros web para detetar configurações incorretas em servidores web e fugas inadvertidas de ficheiros confidenciais antes que agentes maliciosos os possam explorar. Concebido sem dependências externas, executa nativamente em ambientes de consola Python e pipelines de segurança.

***

## Âmbito de Deteção de Superfície e Arquitetura de Ameaças

A exposição involuntária de dados confidenciais na raiz de servidores web constitui um dos vetores de Acesso Inicial mais perigosos na infraestrutura digital. Servidores Nginx, Apache, Caddy ou buckets S3 mal configurados servem frequentemente ficheiros ocultos e cópias de segurança diretamente a utilizadores não autorizados.

* **Variáveis de Ambiente e Segredos**: Verificação exaustiva de ficheiros `.env`, `.env.local` e `.env.production` contendo credenciais de bases de dados, chaves criptográficas e chaves de APIs.
* **Metadados de Controlo de Versões**: Análise de rotas como `/.git/HEAD` e `/.git/config`, prevenindo a reconstrução completa do código-fonte proprietário por agentes externos.
* **Cópias de Segurança de Bases de Dados**: Deteção de ficheiros SQL sensíveis como `backup.sql`, `dump.sql` e `database.sql`, que expõem tabelas relacionais completas.
* **Infraestrutura e Orquestração**: Deteção de configurações de contentores em `docker-compose.yml` e divulgação de parâmetros de diagnóstico via `phpinfo.php` e `config.json`.
* **Credenciais de Acesso a Plataformas Cloud**: Inspeção de caminhos `/.aws/credentials`, evitando a usurpação administrativa de contas em serviços de nuvem.
* **Alinhamento Normativo**: Totalmente alinhado com as orientações do Quadro Nacional de Cibersegurança do CNCS e RGPD (CNPD).

***

## Matriz de Inspeção da Superfície de Ataque

<table>
  <thead>
    <tr>
      <th align="left">Rota Monitorizada</th>
      <th align="left">Severidade</th>
      <th align="left">Vetor de Risco e Tipologia de Ativo</th>
      <th align="left">Medida de Proteção Recomendada</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>/.env</code></td>
      <td>CRÍTICO</td>
      <td>Variáveis de ambiente, palavras-passe de bases de dados e chaves privadas de API.</td>
      <td>Bloquear ficheiros iniciados por ponto no servidor web ou mover para fora da raiz web.</td>
    </tr>
    <tr>
      <td><code>/.env.local</code></td>
      <td>CRÍTICO</td>
      <td>Configurações locais de desenvolvimento e segredos de teste colocados em produção.</td>
      <td>Definir regras estritas no .gitignore e rejeitar pedidos a ficheiros de ponto no reverse proxy.</td>
    </tr>
    <tr>
      <td><code>/.env.production</code></td>
      <td>CRÍTICO</td>
      <td>Credenciais ativas dos sistemas de produção e segredos de serviços terceiros.</td>
      <td>Adotar soluções seguras de gestão de cofres em vez de ficheiros estáticos no disco.</td>
    </tr>
    <tr>
      <td><code>/.git/HEAD</code></td>
      <td>CRÍTICO</td>
      <td>Apontador de branch git que revela a estrutura e ramos ativos do repositório.</td>
      <td>Bloquear acessos à pasta /.git no servidor web e eliminar pastas de controlo de versão no deploy.</td>
    </tr>
    <tr>
      <td><code>/.git/config</code></td>
      <td>CRÍTICO</td>
      <td>Endereços de repositórios remotos, caminhos internos e potenciais tokens de acesso.</td>
      <td>Interditar pedidos a metadados de controlo de versões em todos os domínios alojados.</td>
    </tr>
    <tr>
      <td><code>/backup.sql</code></td>
      <td>CRÍTICO</td>
      <td>Cópia integral da base de dados contendo dados pessoais e credenciais cifradas.</td>
      <td>Guardar backups exclusivamente em armazenamento cloud privado com cifra em repouso.</td>
    </tr>
    <tr>
      <td><code>/dump.sql</code></td>
      <td>CRÍTICO</td>
      <td>Despejo de dados deixado inadvertidamente após operações manuais de migração.</td>
      <td>Implementar procedimentos automáticos de limpeza pós-implementação de versões.</td>
    </tr>
    <tr>
      <td><code>/database.sql</code></td>
      <td>CRÍTICO</td>
      <td>Esquema estrutural da base de dados e dados iniciais de configuração da aplicação.</td>
      <td>Proibir a manutenção de scripts de bases de dados em pastas com acesso web público.</td>
    </tr>
    <tr>
      <td><code>/docker-compose.yml</code></td>
      <td>ALTO</td>
      <td>Topologia de contentores, identificadores de rede interna e variáveis de execução.</td>
      <td>Excluir manifestos de orquestração dos artefactos transferidos para produção.</td>
    </tr>
    <tr>
      <td><code>/phpinfo.php</code></td>
      <td>MÉDIO</td>
      <td>Versão de PHP, extensões ativas, variáveis do sistema e caminhos do servidor.</td>
      <td>Desativar a função phpinfo() no ficheiro php.ini e apagar ficheiros de diagnóstico.</td>
    </tr>
    <tr>
      <td><code>/config.json</code></td>
      <td>ALTO</td>
      <td>Configuração interna da aplicação, endereços de serviços e parâmetros de ligação.</td>
      <td>Restringir a consulta direta a ficheiros JSON ou integrar configurações no código compilado.</td>
    </tr>
    <tr>
      <td><code>/.aws/credentials</code></td>
      <td>CRÍTICO</td>
      <td>Chaves de acesso e segredos operacionais para infraestruturas na AWS.</td>
      <td>Privilegiar perfis de instância e papéis IAM em detrimento de credenciais em disco.</td>
    </tr>
  </tbody>
</table>

***

## Instalação e Guia de Execução Rápida

Requer Python 3.8 ou superior recorrendo apenas a módulos da biblioteca padrão (`sys`, `json`, `ssl`, `argparse`, `urllib.request`). Não são necessárias instalações adicionais através do pip.

Clone o repositório e execute a análise sobre o domínio pretendido:

```bash
git clone https://github.com/TAVARDT-Portugal/exposed-files-scanner-pt.git
cd exposed-files-scanner-pt
python scanner.py exemplo.pt
```

Para extrair os resultados em formato estruturado JSON:

```bash
python scanner.py exemplo.pt -j
```

Inspecione domínios definindo o protocolo de ligação:

```bash
python scanner.py https://exemplo.pt -j
```

Exemplo de payload JSON obtido:

```json
{
  "target": "https://exemplo.pt",
  "status": "Critical (Severe secret or data exposure detected)",
  "inspected_routes": 12,
  "exposed_count": 1,
  "exposed_files": [
    {
      "path": "/.env",
      "url": "https://exemplo.pt/.env",
      "status_code": 200,
      "severity": "CRITICAL",
      "description": "Environment variables and API secrets"
    }
  ],
  "recommendation": "Immediately decommission exposed files and enforce web server hardening controls."
}
```

***

## Perguntas Frequentes (FAQ Técnico)

### De que modo a ferramenta distingue a exposição genuína de respostas de erro 404 personalizadas?
O motor monitoriza a ocorrência de códigos de resposta HTTP 200 OK. Caso o servidor web apresente páginas de erro com código 200 em vez de 404 ou 403, as equipas de sistemas devem normalizar as respostas para evitar falsas notificações de auditoria.

### Que perigos decorrem do acesso público à diretoria `/.git/`?
A exposição de rotas como `/.git/HEAD` e `/.git/config` permite a transferência de objetos do repositório por atacantes através de ferramentas automatizadas. Isto viabiliza a extração integral do código-fonte, inspeção de commits históricos e identificação de credenciais antigas.

### Por que razão o motor de varrimento não valida certificados SSL durante a pesquisa?
O script utiliza um contexto SSL flexível (`ssl.CERT_NONE`) com verificação de nomes desativada para que as equipas possam avaliar domínios de desenvolvimento, certificados autoassinados ou serviços em processo de renovação sem bloqueios de execução.

### Como pode a resposta em formato JSON ser integrada em processos de DevSecOps?
Através do parâmetro `-j`, o script escreve dados JSON válidos no standard output. As esteiras de integração contínua podem analisar o campo `exposed_count` com o utilitário jq, abortando deploys se algum ficheiro crítico for devolvido com código HTTP 200.

### Quais as intervenções imediatas recomendadas para mitigar estas vulnerabilidades?
As equipas devem remover de imediato os ficheiros sensíveis da raiz acessível e aplicar regras de bloqueio no Nginx (`location ~ /\.(?!well-known).* { deny all; }`) ou no Apache (`RedirectMatch 404 /\.(git|env)`). Backups de dados nunca devem residir em diretórios públicos.

***

## Consultoria em Cibersegurança

Para auditorias de segurança ofensiva, testes de penetração e proteção perimetral zero trust em Portugal, contacte a equipa da **TAVARDT Portugal**.

***

## Links Úteis

* **Portal Corporativo:** https://ag.tavardt.com/pt/
* **Suíte de Diagnósticos:** https://tools.tavardt.com/pt/
* **Contacto Técnico:** https://ag.tavardt.com/pt/contacto/

***

### Licença
Distribuído sob a Licença MIT.

***

© 2026 TAVARDT. Todos os direitos reservados.
