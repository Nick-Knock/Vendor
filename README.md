# Vendor
Program to determine the card issuer.
To run program you need to create a new project in Visual Studio (choose Console Application) and insert the code written below.

////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Task_1_Vendor
{
    class Program
    {
        static void Main(string[] args)
        {
            string Num, Vendor;
            Console.WriteLine("Please enter the number of the card:");
            Num = Console.ReadLine();
            Vendor = GetCreditCardVendor(Num);
            Console.WriteLine(Vendor);
            Console.ReadKey();
        }


        static string GetCreditCardVendor(string Num)
        {
            int[] Check_Num = new int[4];
            string Vendor, Error;

            const int AmExpress_JCB = 3, //Перші цифри номеру карток
                      Maestro = 6,
                      Maestro_Master = 5,
                      Visa = 4;

            if (!(Verification(Num))) { Error = "You entered the wrong number."; return Error; } // Перевірка на правильність введеного номеру

            for (int i = 0; i < Check_Num.Length; i++)
            {
                Check_Num[i] = Num[i] - '0'; //Присвоєння елементам масиву перших 4 цифр номера картки
            }

            switch (Check_Num[0]) //Визначення вендора картки за її першими цифрами
            {   
                case AmExpress_JCB:
                    if (Check_Num[1] == 4 || Check_Num[1] == 7)
                    { Vendor="AmericanExpress"; break; }
                    if (Check_Num[1] == 5)
                        if (Check_Num[2] * 10 + Check_Num[3] >= 28 && Check_Num[2] * 10 + Check_Num[3] <= 89)
                        { Vendor = "JCB"; break; }
                    Vendor = "Unknown";
                    break;
                case Maestro:
                    Vendor = "Maestro"; break;
                case Maestro_Master:
                    if (Check_Num[1] == 0 || Check_Num[1] >= 6)
                        Vendor = "Maestro";
                    else
                        Vendor = "MasterCard";
                    break;
                case Visa: Vendor = "Visa"; break;
                default: Vendor = "Unknown"; break;
            }
            return Vendor;
        }


        static bool Verification (string Num)
        {
            if (Num.Length != 19 && Num.Length != 16) return false; //Чи всі цифри введені
            for (int i = 0; i < Num.Length; i++)
                if (Num[i] - '0' < 0 || Num[i] - '0' > 9) //Чи всі введені символи є цифрами
                    if (Num.Length==16) 
                        return false;
                    else 
                        if (!((i==4 || i==9 || i==14) && (Num[i]==' ')))
                           return false;
            return true;
        }

    }
}

