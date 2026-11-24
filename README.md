# Scanner de Ficheiros e Credenciais Expostas

[![Verificador de Ficheiros Expostos](https://img.shields.io/badge/TAVARDT-Superficie%20de%20Ataque-5b3cf5.svg)](https://tools.tavardt.com/pt/verificador-ficheiros-expostos/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Scanner de superficie de ataque para detecao automatizada de ficheiros de ambiente, copias de seguranca, repositorios git e credenciais expostas.

Desenvolvido e mantido pela divisao de ciberseguranca ofensiva da **TAVARDT**.

***

## Ambito de Detecao de Superficie

* **Variaveis de Ambiente e Chaves**: Inspecao de ficheiros `.env`, `.env.local` e `.env.production`.
* **Metadados de Codigo-Fonte**: Verificacao de diretorios `/.git/HEAD` e `/.git/config`.
* **Copias de Seguranca de Dados**: Detecao de despejos SQL como `backup.sql`, `dump.sql` e `database.sql`.
* **Infraestrutura e Orquestracao**: Verificacao de ficheiros `docker-compose.yml` e diagnosticos `phpinfo.php`.
* **Alinhamento Normativo**: Alinhado com as exigencias do Quadro Nacional de Ciberseguranca do CNCS e RGPD (CNPD).

***

## Instalacao e Execucao

Requer Python 3.8 ou superior utilizando exclusivamente a biblioteca padrao.

```bash
git clone https://github.com/TAVARDT-Portugal/exposed-files-scanner-pt.git
cd exposed-files-scanner-pt
python scanner.py empresa.pt
```

Para exportar os resultados em JSON:

```bash
python scanner.py empresa.pt -json
```

***

## Diagnostico Completo na Web

Para auditorias continuas de perimetro, utilize a ferramenta oficial:

* **Verificador de Ficheiros Expostos**
* **[Portal Corporativo TAVARDT](https://ag.tavardt.com/pt/)**

***

## Consultoria em Ciberseguranca

Para projetos de seguranca ofensiva e auditoria de vulnerabilidades em Portugal, contacte a equipa da **TAVARDT**.

***

© 2026 TAVARDT. Todos os direitos reservados.
