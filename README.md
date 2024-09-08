Este projeto implementa a infraestrutura necessária para processar pipelines de dados, realizando processos de ETL (Extract, Transform, Load) e normalização de dados para o banco de dados PostgreSQL, a partir de fontes diversas, como Monday.com. Utiliza o AWS SAM (Serverless Application Model) para facilitar a criação e gerenciamento dos recursos na AWS.

## Arquitetura

A infraestrutura é definida no formato YAML e inclui os seguintes componentes:

- **Amazon RDS for PostgreSQL**: Um banco de dados PostgreSQL que armazena os dados processados pelos pipelines de ETL.
- **AWS Secrets Manager**: Gerencia as credenciais do banco de dados, gerando e armazenando a senha do banco de dados de forma segura.
- **Amazon EC2 Security Group**: Controla o acesso ao banco de dados, limitando as conexões ao PostgreSQL via porta 5432.

## Parâmetros

- **DBSecretName**: O nome do segredo no AWS Secrets Manager que armazena a senha do banco de dados.
- **DBUsername**: O nome de usuário para se conectar ao banco de dados PostgreSQL.

## Recursos

### Banco de Dados RDS

- **RDSCoreInstance**: Um banco de dados PostgreSQL implementado na classe db.t3.micro com 20GB de armazenamento, acessível publicamente (essa configuração deve ser revisada para ambientes de produção).
  
- **RDSCoreSecurityGroup**: Um grupo de segurança que permite o tráfego TCP na porta 5432, atualmente configurado para permitir acesso de qualquer IP (recomenda-se restringir este acesso em ambientes de produção).

- **DBCoreSubnetGroup**: Conjunto de sub-redes onde o RDS está localizado, garantindo alta disponibilidade entre várias zonas de disponibilidade.

### Gerenciamento de Segredos

- **RDSCoreSecret**: Um segredo gerenciado pelo AWS Secrets Manager que armazena de maneira segura a senha do banco de dados PostgreSQL.

## Processos de ETL

O principal objetivo deste projeto é realizar processos de ETL e normalização de dados, convertendo dados de fontes como Monday.com para o banco de dados PostgreSQL. Este pipeline de dados manipula e transforma os dados de acordo com as necessidades de negócio antes de carregá-los no banco de dados.

## Requisitos

- AWS CLI configurado
- AWS SAM CLI instalado
- Python 3.8+
- Docker (para testar e fazer deploy da aplicação)

## Deploy

Para fazer o deploy da infraestrutura e dos pipelines de ETL:

1. Instale o AWS SAM CLI se ainda não o tiver instalado.
2. Clone este repositório.
3. No terminal, navegue até o diretório do projeto e execute o seguinte comando para empacotar e fazer o deploy da infraestrutura:

   ```bash
   sam build
   sam deploy --guided
   ```

4. Durante o deploy, você será solicitado a inserir valores para os parâmetros `DBSecretName` e `DBUsername`.

## Notas Importantes

- **Segurança**: Certifique-se de restringir o acesso ao banco de dados RDS em ambientes de produção. Atualmente, o acesso é permitido de qualquer IP (0.0.0.0/0) para facilitar o desenvolvimento, mas isso deve ser modificado para um intervalo de IPs mais restrito.
- **Substitua valores padrão**: Verifique os valores de sub-rede e o VPC para garantir que a configuração corresponda ao seu ambiente.

## Licença

Este projeto está licenciado sob os termos da MIT License.
```