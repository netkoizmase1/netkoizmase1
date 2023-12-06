using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

class Program
{
    static List<string> predmeti = new List<string> {
        "matematika", "hrvatski", "engleski", "tjelesni", "programiranje", "webdizajn", "bazapodataka"
    };

    static void Main()
    {
        while (true)
        {
            Console.WriteLine("Odaberite opciju:");
            Console.WriteLine("1. Ispis svih ucenika");
            Console.WriteLine("2. Unos novih ocjena");
            Console.WriteLine("3. Pretraga ucenika po ocjeni i predmetu");
            Console.WriteLine("4. Pretraga ucenika po imenu i ispis ocjena");
            Console.WriteLine("5. Izlaz");

            int opcija;
            while (!int.TryParse(Console.ReadLine(), out opcija) || opcija < 1 || opcija > 5)
            {
                Console.WriteLine("Molimo unesite broj 1, 2, 3, 4 ili 5.");
            }

            if (opcija == 1)
            {
                IspisUcenika();
            }
            else if (opcija == 2)
            {
                Console.WriteLine("Unesite ime ucenika:");
                string ime = Console.ReadLine();
                UnosOcjena(ime);
            }
            else if (opcija == 3)
            {
                PretragaUcenika();
            }
            else if (opcija == 4)
            {
                PretragaPoImenu();
            }
            else if (opcija == 5)
            {
                break;
            }
        }
    }

    static void IspisUcenika()
    {
        string[] linije = File.ReadAllLines("ucenici.txt");
        foreach (string linija in linije)
        {
            Console.WriteLine(linija);
        }
    }

    static void UnosOcjena(string ime)
    {
        using (StreamWriter sw = File.AppendText("ucenici.txt"))
        {
            foreach (string predmet in predmeti)
            {
                Console.WriteLine($"Unesite ocjenu iz {predmet}:");
                int ocjena;
                while (!int.TryParse(Console.ReadLine(), out ocjena) || ocjena < 1 || ocjena > 5)
                {
                    Console.WriteLine("Molimo unesite broj izmedu 1 i 5.");
                }

                sw.WriteLine($"{ime}, {predmet}, {ocjena}");
            }
        }
    }

    static void PretragaUcenika()
    {
        Console.WriteLine("Unesite naziv predmeta za pretragu:");
        string predmetPretraga = Console.ReadLine().ToLower();

        if (!predmeti.Contains(predmetPretraga))
        {
            Console.WriteLine("Uneseni predmet ne postoji.");
            return;
        }

        Console.WriteLine("Unesite ocjenu za pretragu:");
        int ocjenaPretraga;
        while (!int.TryParse(Console.ReadLine(), out ocjenaPretraga) || ocjenaPretraga < 1 || ocjenaPretraga > 5)
        {
            Console.WriteLine("Molimo unesite broj izmedu 1 i 5.");
        }

        string[] linije = File.ReadAllLines("ucenici.txt");
        var rezultati = linije.Where(line => line.Contains($"{predmetPretraga}, {ocjenaPretraga}"));
        
        if (rezultati.Any())
        {
            Console.WriteLine("Rezultati pretrage:");
            foreach (var rezultat in rezultati)
            {
                Console.WriteLine(rezultat);
            }
        }
        else
        {
            Console.WriteLine("Nema rezultata za zadane kriterije pretrage.");
        }
    }

    static void PretragaPoImenu()
    {
        Console.WriteLine("Unesite ime ucenika za pretragu:");
        string imePretraga = Console.ReadLine();

        string[] linije = File.ReadAllLines("ucenici.txt");
        var rezultati = linije.Where(line => line.StartsWith(imePretraga));

        if (rezultati.Any())
        {
            Console.WriteLine($"Ocjene za ucenika {imePretraga}:");
            foreach (var rezultat in rezultati)
            {
                Console.WriteLine(rezultat);
            }
        }
        else
        {
            Console.WriteLine("Nema rezultata za zadano ime ucenika.");
        }
    }
}
