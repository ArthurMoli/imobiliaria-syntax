# Imobiliária Syntax

Plataforma de corretagem de imóveis construída em **Django 5** com **Tailwind CSS**, com controle de acesso por perfil, acompanhamento de obras e central de notificações.

Projeto da avaliação final da disciplina de Desenvolvimento Web — 4º semestre de Análise e Desenvolvimento de Sistemas (SENAI).

---

## Funcionalidades

**Dois perfis de usuário, com permissões distintas:**

| | Cliente (`CL`) | Corretor (`CO`) |
|---|:---:|:---:|
| Navegar e filtrar imóveis | ✅ | ✅ |
| Demonstrar interesse em um imóvel | ✅ | — |
| Publicar e editar anúncios | — | ✅ |
| Registrar progresso de obras | — | ✅ |
| Ver interessados nos próprios imóveis | — | ✅ |

- **Catálogo com filtros** — busca por tipo de imóvel, número de quartos e preço máximo
- **Anúncios** — CRUD completo, restrito ao corretor que publicou (`PermissionDenied` para terceiros)
- **Interesses** — clientes marcam imóveis e acompanham em "Meus Interesses"
- **Obras** — imóveis com status *Em Obra* têm timeline de progresso registrada pelo corretor
- **Notificações** — central com marcação individual ou em lote
- **Perfil** — edição de dados de contato e troca de senha
- **Recuperação de senha** — fluxo completo via e-mail

---

## Stack

| Camada | Tecnologia |
|---|---|
| Backend | Django 5.2 (Python 3.10+) |
| Frontend | Django Templates + Tailwind CSS |
| Banco | SQLite |
| Filtros | django-filter |
| Formulários | django-widget-tweaks |
| Imagens | Pillow |

Usuário customizado (`AbstractUser`) com campos `perfil` e `telefone`.

---

## Rodando localmente

**Pré-requisitos:** Python 3.10 ou superior (o Django 5.2 não roda em versões anteriores) e Node.js, caso queira recompilar o CSS.

```bash
git clone https://github.com/ArthurMoli/AvFinalDWEBSEM4.git
cd AvFinalDWEBSEM4

python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

A aplicação sobe em **http://127.0.0.1:8000**.

O `db.sqlite3` não é versionado — o `migrate` cria um banco limpo. Crie usuários pelo `/admin/` ou pela tela de cadastro, escolhendo o perfil desejado.

### Recompilando o CSS

O `static/output.css` já vem compilado. Para alterar estilos:

```bash
npm install
npx tailwindcss -i ./static/input.css -o ./static/output.css --watch
```

---

## Estrutura

```
config/        Configuração do projeto (settings, urls)
users/         Usuário customizado, cadastro e perfil
properties/    Imóveis, anúncios e interesses
works/         Acompanhamento de obras
core/          Home e notificações
reports/       Dashboard
templates/     Templates de todos os apps
static/        CSS do Tailwind (input e output)
```

Os templates ficam centralizados em `templates/`, organizados por app.

---

## Rotas

| Rota | Descrição |
|---|---|
| `/` | Home com imóveis recentes |
| `/imoveis/` | Catálogo com filtros |
| `/imoveis/<id>/` | Detalhe do imóvel |
| `/imoveis/novo/` | Novo anúncio *(corretor)* |
| `/imoveis/meus-imoveis/` | Anúncios do corretor |
| `/imoveis/meus-interesses/` | Imóveis marcados *(cliente)* |
| `/works/` | Obras em andamento *(corretor)* |
| `/users/perfil/` | Perfil do usuário |
| `/notificacoes/` | Central de notificações |
| `/admin/` | Django admin |

---

## Notas

As configurações de `SECRET_KEY` e `DEBUG` estão fixas em `config/settings.py` com valores de desenvolvimento. Para uso em produção, mova-as para variáveis de ambiente e defina `DEBUG = False`.
