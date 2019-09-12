# Atividade 07

Nesta atividade iremos configurar os pipelines de build e de release para executar os scripts de reorganização do Genexus.

### Tabela de Controle

Para controlar a execução dos scripts sql iremos criar uma tabela de controle na base de dados da aplicação. Conecte na VM e abra o SQL Server Management Studio e execute o script abaixo para criar a tabela na base de dados.

```sql 
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

SET ANSI_PADDING ON
GO

CREATE TABLE [dbo].[AzureDevOpsMigration]
(
                [Id] [int] IDENTITY(1,1) NOT NULL,
                [DataCriacao] [datetime] NOT NULL CONSTRAINT [DF_AzureDevOpsMigration_DataCriacao]  DEFAULT (getdate()),
                [DataExecucao] [datetime] NULL,
                [NomeArquivo] [varchar](100) NOT NULL,
                [ScriptSQL] [varchar](max) NOT NULL,
                CONSTRAINT [PK_AzureDevOpsMigration] PRIMARY KEY CLUSTERED 
(
                [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

GO

SET ANSI_PADDING OFF
GO

```

![sql management](../imagens/sqlscript1.png)

Criar uma pasta chamada **scriptsql** na pasta do environment do .NET. Nesta pasta iremos armazenar os scripts sql que serão executados pelo Azure Pipeline.

![pasta sql](../imagens/sqlscript2.png)

Agora vamos criar mais um atributo na transação cliente, para gerarmos uma reorganização no Genexus. Abra a transação Cliente, e crie um atributo chamado ClienteSobrenome Varchar(40), e salve a alteração.

![atributo](../imagens/sqlscript3.png)

Impactar as tabelas para gerar a reorganização.

![impact](../imagens/sqlscript4.png)

Copiar as instruções SQL geradas e criar um arquivo chamado **addcoluna.sql**. Salvar esse arquivo na pasta **scriptsql**.

![reorg](../imagens/sqlscript5.png)

![script](../imagens/sqlscript6.png)

![script](../imagens/sqlscript7.png)

Abra o Git CMD e se posicione no diretório do environment .NET.

![gitcmd](../imagens/sqlscript8.png)

Vamos executar o comando commit para que os arquivos sejam armazenados no repositório.

```bash
git commit -a -m 'script'
```

![git commit](../imagens/sqlscript9.png)

Executar o comando push para subir os arquivos para o repositório do Azure Repos.

```bash
git push origin master
```

![git commit](../imagens/sqlscript10.png)

Acesse o Azure Repos para verificar se a pasta scriptsql e o arquivo addcoluna.sql está lá.

![azure repos](../imagens/sqlscript11.png)

Agora vamos alterar o Pipeline de Build para pegar o arquivo sql e transformar num artefato para o Pipeline de Release.
Clique no item **Builds** para listar os pipelines. Clicar na opção **Edit** para abrir o pipeline para edição.

![edit build](../imagens/sqlscript12.png)

Clicar no **+** no item **Agent job 1**. Preencher o campo de pesquisa com **Archive** e selecinar o item **Archive files** e clique em **Add**.

![edit build](../imagens/sqlscript13.png)

Próxima atividade: [Atividade 07](atividades/07-atividade.md)



