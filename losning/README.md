## Fyra lösningsvarianter:

Följande är ett relativt "simpelt" exempel på FIL-hantering, som visar olika varianter utan och med felhantering. Oavsett hur det går med whiskeyn, så är "fjärde försöket" att föredra! :)

___Program.cs___

```c#
using System;
using System.IO;

namespace UsingFiles
{
    class Program
    {
        static void Main(string[] args)
        {
            Program p = new Program();
            p.Run();
        }

        private void Run()
        {
            FirstTry();
            SecondTry();
            ThirdTry();
            FourthTry();
        }

        private void FirstTry()
        {
            // Felhantering saknas!
            StreamReader reader = new StreamReader(@"..\..\12 flaskor whiskey.txt");
            string line = null;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
                Console.WriteLine();
            }
            reader.Close();
        }

        private void SecondTry()
        {
            // Felhantering med finally-block som garanterar att filen
            // stängs, om den är öppen, vad som än händer.
            StreamReader reader = null;
            try
            {
                reader = new StreamReader(@"..\..\12 flaskor whiskey.txt");
                string line = null;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                    Console.WriteLine();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Ett oväntat fel inträffade.");
                Console.WriteLine(e.Message);
            }
            finally
            {
                if (reader != null)
                {
                    reader.Close();
                }
            }
        }

        // Felhantering saknas, men eftersom using används kommer alltid filen att stängas
        // vad som än händer.
        private void ThirdTry()
        {
            using (StreamReader reader = new StreamReader(@"..\..\12 flaskor whiskey.txt"))
            {
                string line = null;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                    Console.WriteLine();
                }
            }
        }

        // using + felhantering
        private void FourthTry()
        {
            using (StreamReader reader = new StreamReader(@"..\..\12 flaskor whiskey.txt"))
            {
                try
                {
                    string line = null;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                        Console.WriteLine();
                    }

                }
                catch (Exception e)
                {
                    Console.WriteLine("Ett oväntat fel inträffade.");
                    Console.WriteLine(e.Message);
                }
            }
        }
    }
}
```
