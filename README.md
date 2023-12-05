# Library
#Basic online library made by me



import datetime


class Livro:
    def __init__(self, titulo, autor, isbn, categoria):
        self.titulo = titulo
        self.autor = autor
        self.isbn = isbn
        self.status = "Disponível"
        self.data_emprestimo = None
        self.categoria = categoria

    def definir_status(self, status):
        self.status = status

    def obter_status(self):
        return self.status

    def registrar_emprestimo(self):
        self.data_emprestimo = datetime.datetime.now()

    def verificar_data_devolucao(self):
        if self.data_emprestimo is not None:
            data_devolucao = self.data_emprestimo + datetime.timedelta(days=10)
            return data_devolucao.strftime("%d/%m/%Y")
        else:
            return "Livro não emprestado"


class Usuario:
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade
        self.livros_emprestados = []

    def emprestar_livro(self, livro):
        self.livros_emprestados.append(livro)
        livro.definir_status("Emprestado")
        livro.registrar_emprestimo()
        print(f"{livro.titulo} foi emprestado com sucesso para {self.nome}.")
        print(f"Você tem até {livro.verificar_data_devolucao()} para devolver o livro.")

    def devolver_livro(self, livro):
        if livro in self.livros_emprestados:
            self.livros_emprestados.remove(livro)
            livro.definir_status("Disponível")
            print(f"{livro.titulo} foi devolvido com sucesso por {self.nome}.")
        else:
            print("Você não tem esse livro emprestado.")

    def listar_livros_emprestados(self):
        if self.livros_emprestados:
            print(f"Livros emprestados por {self.nome}:")
            for i, livro in enumerate(self.livros_emprestados, 1):
                print(f"{i}. {livro.titulo} - {livro.autor}")
        else:
            print(f"{self.nome} não tem livros emprestados no momento.")


class Biblioteca:
    def __init__(self):
        self.lista_de_livros = []

    def adicionar_livro(self, livro):
        self.lista_de_livros.append(livro)

    def listar_livros_disponiveis(self, categoria=None):
        if categoria:
            livros_disponiveis = [livro for livro in self.lista_de_livros if
                                  livro.obter_status() == "Disponível" and livro.categoria == categoria]
        else:
            livros_disponiveis = [livro for livro in self.lista_de_livros if livro.obter_status() == "Disponível"]

        if not livros_disponiveis:
            print("Não há livros disponíveis no momento.")
        else:
            print("Livros disponíveis:")
            for i, livro in enumerate(livros_disponiveis, 1):
                print(f"{i}. {livro.titulo} - {livro.autor} ({livro.categoria})")

    def emprestar_livro(self, escolha, usuario):
        livros_disponiveis = [livro for livro in self.lista_de_livros if livro.obter_status() == "Disponível"]
        if not livros_disponiveis:
            print("Não há livros disponíveis para empréstimo.")
            return

        try:
            escolha = int(escolha)
            if escolha < 1 or escolha > len(livros_disponiveis):
                print("Escolha um número válido.")
                return

            livro_escolhido = livros_disponiveis[escolha - 1]
            usuario.emprestar_livro(livro_escolhido)
        except ValueError:
            print("Escolha inválida. Insira o número correspondente ao livro desejado.")

    def devolver_livro(self, titulo, usuario):
        for livro in self.lista_de_livros:
            if livro.titulo == titulo and livro.obter_status() == "Emprestado":
                usuario.devolver_livro(livro)
                return
        print("Livro não encontrado ou não foi emprestado.")


if __name__ == "__main__":
    minha_biblioteca = Biblioteca()

    nome_usuario = input("Digite seu nome: ")
    idade_usuario = input("Digite sua idade: ")

    usuario1 = Usuario(nome_usuario, idade_usuario)

    livro1 = Livro("Python for Beginners", "John Smith", "12345", "Programação")
    livro2 = Livro("Data Science Handbook", "Jane Doe", "67890", "Ciência de Dados")
    livro3 = Livro("LINGUAGEM C: COMPLETA E DESCOMPLICADA", "André Backes", "2340", "Programação")
    livro4 = Livro("Orientação a Objetos: Aprenda seus conceitos e suas aplicabilidades de forma efetiva",
                   "Thiago Leite e Carvalho", "6890", "Programação")
    livro5 = Livro("Java Como Programar", "Paul J. Deitel", "5678", "Programação")
    livro6 = Livro("Fundamentos matemáticos para a ciência da computação: Matemática Discreta e Suas Aplicações",
                   "Judith L. Gersting", "9876", "Matemática")
    livro7 = Livro("ORIENTAÇÃO A OBJETOS", "Caio de Melo", "5432", "Programação")
    livro8 = Livro("Linguagens Formais e Autômatos", "Paulo Blauth", "1234", "Ciência da Computação")

    minha_biblioteca.adicionar_livro(livro1)
    minha_biblioteca.adicionar_livro(livro2)
    minha_biblioteca.adicionar_livro(livro3)
    minha_biblioteca.adicionar_livro(livro4)
    minha_biblioteca.adicionar_livro(livro5)
    minha_biblioteca.adicionar_livro(livro6)
    minha_biblioteca.adicionar_livro(livro7)
    minha_biblioteca.adicionar_livro(livro8)

    while True:
        print("\nOpções:")
        print("1. Listar todas as categorias de livros")
        print("2. Listar livros disponíveis por categoria")
        print("3. Emprestar livro")
        print("4. Devolver livro")
        print("5. Listar livros emprestados por usuário")
        print("6. Sair")
        escolha = input("Escolha uma opção: ")

        if escolha == "1":
            categorias = set(livro.categoria for livro in minha_biblioteca.lista_de_livros)
            print("Categorias disponíveis:")
            for i, categoria in enumerate(categorias, 1):
                print(f"{i}. {categoria}")
        elif escolha == "2":
            categorias = set(livro.categoria for livro in minha_biblioteca.lista_de_livros)
            print("Escolha uma categoria:")
            for i, categoria in enumerate(categorias, 1):
                print(f"{i}. {categoria}")
            escolha_categoria = input("Escolha uma categoria: ")
            try:
                escolha_categoria = int(escolha_categoria)
                if escolha_categoria < 1 or escolha_categoria > len(categorias):
                    print("Escolha uma categoria válida.")
                else:
                    categoria_escolhida = list(categorias)[escolha_categoria - 1]
                    minha_biblioteca.listar_livros_disponiveis(categoria_escolhida)
            except ValueError:
                print("Escolha inválida. Insira o número correspondente à categoria desejada.")
        elif escolha == "3":
            minha_biblioteca.listar_livros_disponiveis()
            livro_escolhido = input("Escolha o número do livro que deseja emprestar: ")
            minha_biblioteca.emprestar_livro(livro_escolhido, usuario1)

        elif escolha == "4":
            livro_a_devolver = input("Digite o título do livro que deseja devolver: ")
            minha_biblioteca.devolver_livro(livro_a_devolver, usuario1)
        elif escolha == "5":
            usuario1.listar_livros_emprestados()
        elif escolha == "6":
            print("Saindo do programa. Obrigado!")
            break
        else:
            print("Opção inválida. Tente novamente.")

