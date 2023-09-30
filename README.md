# IBID_INTERN_PROGRAM
using System;
using System.Collections.Generic;
using System.Linq;

namespace MinhaAplicacao
{
    public class Produto
    {
        public string Id { get; set; }
        public string Nome { get; set; }
    }

    public class ProdutoRepository
    {
        private List<Produto> produtos;
        private string proximoId;

        public ProdutoRepository()
        {
            produtos = new List<Produto>();
            proximoId = "A";
        }

        public void AdicionarProduto(string nome)
        {
            if (!string.IsNullOrWhiteSpace(nome)) // Verifica se o nome não está em branco ou nulo
            {
                var produto = new Produto { Id = proximoId, Nome = nome };
                produtos.Add(produto);
                proximoId = IncrementarId(proximoId);
            }
            else
            {
                Console.WriteLine("Digite um nome de produto válido.");
                Console.ReadLine(); // Aguarda uma tecla antes de continuar
            }
        }

        public void RemoverProduto(string id)
        {
            var produto = produtos.FirstOrDefault(p => p.Id == id);
            if (produto != null)
            {
                produtos.Remove(produto);
            }
        }

        public void EditarNomeProduto(string id, string novoNome)
        {
            var produto = produtos.FirstOrDefault(p => p.Id == id);
            if (produto != null)
            {
                produto.Nome = novoNome;
            }
        }

        public List<Produto> ObterTodosProdutos()
        {
            return produtos;
        }

        private string IncrementarId(string id)
        {
            char[] chars = id.ToCharArray();
            int index = chars.Length - 1;

            while (index >= 0)
            {
                if (chars[index] < 'Z')
                {
                    chars[index]++;
                    return new string(chars);
                }

                chars[index] = 'A';
                index--;
            }

            // Se todos os caracteres forem 'Z', acrescenta um novo 'A' no início.
            return "A" + new string(chars);
        }
    }

    class Program
    {
        static void Main()
        {
            var produtoRepository = new ProdutoRepository();

            while (true)
            {
                Console.Clear(); // Limpa a tela a cada iteração para exibir o menu

                Console.WriteLine("Escolha uma opção:");
                Console.WriteLine("1 - Adicionar Produto");
                Console.WriteLine("2 - Remover Produto por ID");
                Console.WriteLine("3 - Editar Nome de Produto por ID");
                Console.WriteLine("4 - Visualizar Todos os Produtos");
                Console.WriteLine("5 - Sair");

                var escolha = Console.ReadLine();

                switch (escolha)
                {
                    case "1":
                        Console.Write("Digite o nome do produto: ");
                        var nome = Console.ReadLine();
                        produtoRepository.AdicionarProduto(nome);
                        break;

                    case "2":
                        Console.Write("Digite o ID do produto a ser removido: ");
                        var idRemover = Console.ReadLine();
                        produtoRepository.RemoverProduto(idRemover);
                        break;

                    case "3":
                        Console.Write("Digite o ID do produto a ser editado: ");
                        var idEditar = Console.ReadLine();
                        Console.Write("Digite o novo nome: ");
                        var novoNome = Console.ReadLine();
                        produtoRepository.EditarNomeProduto(idEditar, novoNome);
                        break;

                    case "4":
                        Console.WriteLine("Produtos cadastrados:");
                        var produtos = produtoRepository.ObterTodosProdutos();
                        foreach (var produto in produtos)
                        {
                            Console.WriteLine($"ID: {produto.Id}, Nome: {produto.Nome}");
                        }
                        break;

                    case "5":
                        Environment.Exit(0);
                        break;

                    default:
                        Console.WriteLine("Opção inválida. Pressione Enter para continuar.");
                        Console.ReadLine();
                        break;
                }
            }
        }
    }
}

