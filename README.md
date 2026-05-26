using System;
using System.Collections.Generic;

class Ingresso
{
    public string NomeCliente { get; set; }
    public string Filme { get; set; }
    public int NumeroPoltrona { get; set; }
    public double Valor { get; set; }
    public bool MeiaEntrada { get; set; }
}

class Program
{
    static List<Ingresso> ingressos = new List<Ingresso>();

    static void Main()
    {
        int opcao;
        do
        {
            Console.WriteLine("\n=== CINEMA ===");
            Console.WriteLine("1 - Cadastrar ingresso");
            Console.WriteLine("2 - Listar ingressos vendidos");
            Console.WriteLine("3 - Buscar ingressos por cliente");
            Console.WriteLine("4 - Total arrecadado");
            Console.WriteLine("5 - Sair");
            Console.Write("Escolha: ");
            opcao = int.Parse(Console.ReadLine());

            switch (opcao)
            {
                case 1: CadastrarIngresso(); break;
                case 2: ListarIngressos(); break;
                case 3: BuscarPorCliente(); break;
                case 4: TotalArrecadado(); break;
            }
        } while (opcao != 5);
    }

    static void CadastrarIngresso()
    {
        var ing = new Ingresso();

        Console.Write("Nome do cliente: ");
        ing.NomeCliente = Console.ReadLine();

        Console.Write("Nome do filme: ");
        ing.Filme = Console.ReadLine();

        Console.Write("Número da poltrona: ");
        ing.NumeroPoltrona = int.Parse(Console.ReadLine());

        // Verifica se poltrona já foi vendida para o mesmo filme
        bool ocupada = ingressos.Exists(i =>
            i.NumeroPoltrona == ing.NumeroPoltrona &&
            i.Filme.Equals(ing.Filme, StringComparison.OrdinalIgnoreCase));

        if (ocupada)
        {
            Console.WriteLine("❌ Poltrona já vendida para esse filme!");
            return;
        }

        Console.Write("Valor do ingresso: R$ ");
        ing.Valor = double.Parse(Console.ReadLine());

        Console.Write("Meia entrada? (s/n): ");
        ing.MeiaEntrada = Console.ReadLine().ToLower() == "s";

        ingressos.Add(ing);
        Console.WriteLine("✅ Ingresso cadastrado com sucesso!");
    }

    static void ListarIngressos()
    {
        if (ingressos.Count == 0)
        {
            Console.WriteLine("Nenhum ingresso cadastrado.");
            return;
        }

        foreach (var i in ingressos)
        {
            Console.WriteLine("\n--------------------------");
            Console.WriteLine($"Cliente:  {i.NomeCliente}");
            Console.WriteLine($"Filme:    {i.Filme}");
            Console.WriteLine($"Poltrona: {i.NumeroPoltrona}");
            Console.WriteLine($"Valor:    R$ {i.Valor:F2}");
            Console.WriteLine($"Tipo:     {(i.MeiaEntrada ? "Meia entrada" : "Inteira")}");
        }
    }

    static void BuscarPorCliente()
    {
        Console.Write("Nome do cliente: ");
        string nome = Console.ReadLine();

        var resultado = ingressos.FindAll(i =>
            i.NomeCliente.Equals(nome, StringComparison.OrdinalIgnoreCase));

        if (resultado.Count == 0)
        {
            Console.WriteLine("Nenhum ingresso encontrado.");
            return;
        }

        foreach (var i in resultado)
        {
            Console.WriteLine($"\nFilme: {i.Filme} | Poltrona: {i.NumeroPoltrona} | R$ {i.Valor:F2}");
        }
    }

    static void TotalArrecadado()
    {
        double total = 0;
        foreach (var i in ingressos) total += i.Valor;
        Console.WriteLine($"\nTotal arrecadado: R$ {total:F2}");
    }
}

