# Scanner de Ficheiros e Credenciais Expostas

[![Verificador de Ficheiros Expostos](https://img.shields.io/badge/TAVARDT-Superficie%20de%20Ataque-5b3cf5.svg)](https://tools.tavardt.com/pt/verificador-ficheiros-expostos/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Scanner de superficie de ataque para deteção automatizada de ficheiros de ambiente, copias de seguranca, repositorios git e credenciais expostas.

***

## Ambito de Deteção de Superficie

* **Variaveis de Ambiente e Chaves**: Inspeção de ficheiros `.env`, `.env.local` e `.env.production`.
* **Metadados de Codigo-Fonte**: Verificacao de diretorios `/.git/HEAD` e `/.git/config`.
* **Copias de Seguranca de Dados**: Deteção de despejos SQL como `backup.sql`, `dump.sql` e `database.sql`.
* **Infraestrutura e Orquestracao**: Verificacao de ficheiros `docker-compose.yml` e diagnosticos `phpinfo.php`.
* **Alinhamento Normativo**: Alinhado com as exigencias do Quadro Nacional de Ciberseguranca do CNCS e RGPD (CNPD).

***

## Instalação e Execucao

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

## Consultoria em Ciberseguranca

Para projetos de seguranca ofensiva e auditoria de vulnerabilidades em Portugal, contacte a equipa da **TAVARDT Portugal**.

***

## Links Úteis

* **Portal Corporativo:** https://ag.tavardt.com/pt/
* **Suíte de Diagnósticos:** https://tools.tavardt.com/pt/
* **Contacto Técnico:** https://ag.tavardt.com/pt/contacto/

***

### Licença
Distribuído sob a Licença MIT.

***

© 2026 TAVARDT Portugal. Todos os direitos reservados. Soberania e Defesa de Infraestrutura.
