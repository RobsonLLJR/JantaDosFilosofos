using System;
using System.Text;
using System.Threading;
using System.Collections;
using System.Collections.Generic;


namespace JantarDosFilosofos
{
    class Program
    {
        public static void Main(string[] args)
        {
            Random rnd = new Random();
            int idFilosofo = 0;
            Thread[] filosofos = new Thread[5];
            int[] garfos = new int[] { 1, 1, 1, 1, 1 }; // 1 - Livre / 0 - Em Uso
            int[] pratos = new int[] { 1, 1, 1, 1, 1 }; //1 - Comeu Recentemente / 0 - Terminou de pensar
            for (int i = 0; i < filosofos.Length; i++)
            {
                filosofos[i] = new Thread(new ThreadStart(() => comecar(idFilosofo++, garfos, pratos)));
                filosofos[i].Start();
            }
        }

        static void pensar(int idFilosofo, int[] pratos)
        {
            Random rnd = new Random();
            int tempoPensar = rnd.Next(100, 10000);
            Console.WriteLine("Filosofo " + (idFilosofo + 1) + " pensando...");
            Thread.Sleep(tempoPensar);
            int pratoDoFilosofo = (idFilosofo + 1) % pratos.Length;
            pratos[pratoDoFilosofo] = 0;
        }

        static void comer(int idFilosofo, int[] garfos, int[] pratos)
        {
            Random rnd = new Random();
            int posGarfoEsquerdo = idFilosofo;
            int posGarfoDireito = (idFilosofo + 1) % garfos.Length;

            Console.WriteLine("Filosofo " + (idFilosofo + 1) + " verificando os garfos!");
            if (verficarGarfos(garfos, posGarfoEsquerdo, posGarfoDireito, idFilosofo, pratos))
            {
                Console.WriteLine("Filosofo " + (idFilosofo + 1) + " comendo...");
                Thread.Sleep(rnd.Next(100, 10000));
                garfos[posGarfoEsquerdo] = 1;
                garfos[posGarfoDireito] = 1;
                Console.WriteLine("Filosofo " + (idFilosofo + 1) + " terminou de comer!");
                pratos[posGarfoDireito] = 1;
            }
        }

        static void comecar(int idFilosofo, int[] garfos, int[] pratos)
        {
            while (true)
            {
                comer(idFilosofo, garfos, pratos);
                pensar(idFilosofo, pratos);
            }
        }

        static bool verficarGarfos(int[] garfos, int GarfoEsquerdo, int GarfoDireito, int idFilosofo, int[] pratos)
        {
            //Lock - resolve o problema de deadlock
            lock (garfos)
            {
                if (garfos[GarfoEsquerdo] == 1 && garfos[GarfoDireito] == 1)
                {
                    //Se os garfos estiverem disponível, verificando se comeu recentemente..
                    VerificarSeComeuRecentemente(pratos, GarfoDireito);
                    garfos[GarfoEsquerdo] = 0;
                    garfos[GarfoDireito] = 0;
                    return true;
                }
                else return true;
            }
        }

        //Aqui verifico se o Filosofo comeu recentemente, se sim carrego a Thread com um número randomico menor - proposta de solução de STARVATION
        static void VerificarSeComeuRecentemente(int[] pratos, int idFilosofo)
        {
            Random rnd = new Random();
            int pratoDoFilosofo = (idFilosofo + 1) % pratos.Length;
            while (pratos[pratoDoFilosofo] == 1)
            {
                Thread.Sleep(rnd.Next(100, 5000));
                Console.WriteLine("Filosofo " + (idFilosofo + 1) + " pronto pra comendo...");
                pratos[pratoDoFilosofo] = 0;
            }
        }
    }
}
