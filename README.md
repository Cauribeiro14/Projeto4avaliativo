Projeto 4 — API de Gerenciamento de Filmes
Alunas: Ana Beatriz Viera e Claudiane Ribeiro

1. Apresentação
O projeto consiste no desenvolvimento de uma API REST para gerenciamento de filmes e gêneros cinematográficos, utilizando Python, Django, Django REST Framework e django-filter.

A aplicação permite consultar, cadastrar, atualizar e excluir filmes e gêneros. As consultas são públicas, enquanto as operações de criação, edição e exclusão exigem autenticação por token.

2. Tecnologias utilizadas
Python

Django

Django REST Framework

django-filter

Banco de dados e arquivo CSV para importação dos registros.

3. Configuração do ambiente
Abra o terminal na pasta do projeto, onde está localizado o arquivo manage.py, e execute os comandos abaixo para criar e ativar o ambiente virtual:

python -m venv venv
venv\Scripts\activate
Instale as dependências:

pip install -r requirements.txt

Para ambientes de produção, configure a variável de ambiente `DJANGO_SECRET_KEY` com uma chave secreta própria. A chave padrão do projeto é somente para desenvolvimento local.

Caso o arquivo requirements.txt não esteja disponível, instale as bibliotecas individualmente:

pip install django djangorestframework django-filter
4. Preparação do banco de dados
Execute as migrações para preparar as tabelas da aplicação:

python manage.py makemigrations
python manage.py migrate

O banco local `db.sqlite3` não é versionado; as migrações e o arquivo CSV permitem prepará-lo novamente.
Para importar os dados iniciais, verifique se os arquivos base_filmes.csv e importarDados.py estão na pasta do arquivo manage.py. Em seguida, execute:

python importarDados.py
O script realiza a importação dos filmes e cadastra os gêneros necessários, conforme sua implementação.

5. Inicialização e acesso
Para iniciar o servidor local, utilize:

python manage.py runserver
Após iniciar o servidor, os endereços disponíveis são:

Recurso	Endereço
API navegável	http://127.0.0.1:8000/api/
Painel administrativo	http://127.0.0.1:8000/admin/
O projeto disponibiliza as seguintes credenciais para testes, desde que o banco de dados fornecido contenha o usuário:

Usuário: aluno1

Senha: 1234

Caso seja necessário criar um novo administrador, execute:

python manage.py createsuperuser
6. Estrutura do banco de dados
A aplicação possui duas entidades principais:

Gêneros

nome: nome da categoria cinematográfica.

Filmes

titulo: título do filme.

descricao: descrição da obra.

ano_lancamento: ano de lançamento.

genero: relacionamento com a tabela de gêneros.

duracao: duração do filme.

O relacionamento entre as entidades permite associar cada filme ao seu gênero correspondente.

7. Endpoints da API
Método	Rota	Descrição
GET	/api/generos/	Lista os gêneros cadastrados.
POST	/api/generos/	Cadastra um gênero.
GET	/api/generos/{id}/	Consulta um gênero específico.
PUT/PATCH	/api/generos/{id}/	Atualiza um gênero.
DELETE	/api/generos/{id}/	Exclui um gênero.
GET	/api/filmes/	Lista os filmes cadastrados.
POST	/api/filmes/	Cadastra um filme.
GET	/api/filmes/{id}/	Consulta um filme específico.
PUT/PATCH	/api/filmes/{id}/	Atualiza um filme.
DELETE	/api/filmes/{id}/	Exclui um filme.
POST	/api/token/	Solicita um token de autenticação.
O identificador {id} representa o número correspondente ao registro desejado.

8. Filtros de pesquisa
Com o django-filter, é possível realizar consultas específicas por meio de parâmetros na URL.

Exemplos:

Buscar um filme pelo título: /api/filmes/?titulo=Parasita

Buscar filmes por gênero: /api/filmes/?genero__nome=Animação

Filtrar pelo ano de lançamento: /api/filmes/?ano_lancamento=1999

Combinar gênero e duração: /api/filmes/?genero=1&duracao=169

Pesquisar um gênero: /api/generos/?nome=Drama

A disponibilidade de cada filtro depende da configuração da API.

9. Exemplos de requisições
Consultar filmes
A consulta não exige autenticação:

curl.exe http://127.0.0.1:8000/api/filmes/
Obter um token
curl.exe -X POST http://127.0.0.1:8000/api/token/ ^
  -H "Content-Type: application/json" ^
  -d "{\"username\":\"aluno1\",\"password\":\"1234\"}"
Cadastrar um filme
Substitua SEU_TOKEN_AQUI pelo token recebido:

curl.exe -X POST http://127.0.0.1:8000/api/filmes/ ^
  -H "Authorization: Token SEU_TOKEN_AQUI" ^
  -H "Content-Type: application/json" ^
  -d "{\"titulo\":\"Interestelar\",\"descricao\":\"Uma exploração espacial.\",\"ano_lancamento\":2014,\"genero\":1,\"duracao\":169}"
Atualizar um filme
curl.exe -X PATCH http://127.0.0.1:8000/api/filmes/3/ ^
  -H "Authorization: Token SEU_TOKEN_AQUI" ^
  -H "Content-Type: application/json" ^
  -d "{\"duracao\":170}"
Excluir um filme
curl.exe -X DELETE http://127.0.0.1:8000/api/filmes/3/ ^
  -H "Authorization: Token SEU_TOKEN_AQUI"
Cadastrar um gênero
curl.exe -X POST http://127.0.0.1:8000/api/generos/ ^
  -H "Authorization: Token SEU_TOKEN_AQUI" ^
  -H "Content-Type: application/json" ^
  -d "{\"nome\":\"Documentário\"}"
Os exemplos de cadastro, edição e exclusão dependem de um token válido e de identificadores existentes no banco de dados.

10. Considerações finais
O desenvolvimento desta API permite aplicar conhecimentos de programação com Python, construção de serviços REST, manipulação de dados relacionais, filtros de pesquisa e autenticação.

A utilização do Django REST Framework facilita a organização dos endpoints e oferece uma interface navegável para testar as funcionalidades implementadas.

Projeto acadêmico desenvolvido por Ana Beatriz Viera e Claudiane Ribeiro.