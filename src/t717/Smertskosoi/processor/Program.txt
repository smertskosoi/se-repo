using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Diagnostics;
using System.Data;
using System.Threading;
namespace Задание_до_сб_23_59
{
    class Program
    {
        public static void Main(string[] args)
        {
            Operations operations = new Operations();
            operations._Operations();
        }
    }
    class Operations
    {
        public void _Operations()
        {
            Console.WriteLine("uid - 1 \n pid - 2 \n cpu - 3 \n ram - 4");
            while (true)
            {
                int a = Convert.ToInt32(Console.ReadLine());
                switch (a)
                {
                    case 1:
                        {
                            Uid_Name_write();
                            break;
                        }
                    case 2:
                        {
                            Pid_write();
                            break;
                        }
                    case 3:
                        {
                            Cpu_Write();
                            break;
                        }
                    case 4:
                        {
                            Ram_Write();
                            break;
                        }
                    default:
                        Console.WriteLine("Введите команду корректно");
                        break;
                }
            }
        }
        public void Uid_Name_write()
        {
            Console.WriteLine("\n" + Environment.MachineName);
            Console.WriteLine(System.Security.Principal.WindowsIdentity.GetCurrent().Name + "\n");
        }
        public void Pid_write()
        {
            Process[] processlist = Process.GetProcesses();
            foreach (Process theprocess in processlist)
            {
                Console.WriteLine("{0} ID: {1}", theprocess.ProcessName, theprocess.Id);
            }
        }
        public void Cpu_Write()
        {
            PerformanceCounter _cpuCounter = new PerformanceCounter("Processor", "% Processor Time", "_Total");
            int Count = (int)_cpuCounter.NextValue();
            char[] arr = new char[10];
            for (int i = 0; i < 10; i++)
            {
                arr[i] = '-';
            }
            while (Console.KeyAvailable == false)
            {
                Console.WriteLine("Нажмите любую цифру, чтобы вернуться");
                Console.WriteLine("{0}%", Count);
                for (int i = 0; i < 10; i++)
                {
                    if (i <= (Count / 10 - 1))
                    {
                        arr[i] = '|';
                    }
                    else
                    {
                        arr[i] = '-';
                    }
                    Console.Write(arr[i]);
                }
                Console.WriteLine();
                Count = (int)_cpuCounter.NextValue();
                Thread.Sleep(1000);
                Console.Clear();
            }
            _Operations();
        }
        public void Ram_Write()
        {
            PerformanceCounter _ramPercent = new PerformanceCounter("Memory", "% Committed Bytes In Use");
            PerformanceCounter _ramCounter = new PerformanceCounter("Memory", "Available MBytes");
            int Track_Memory = (int)_ramPercent.NextValue();
            char[] arr = new char[10];
            for (int i = 0; i < 10; i++)
            {
                arr[i] = '-';
            }
            while (Console.KeyAvailable == false)
            {
                Console.WriteLine("Нажмите любую цифру, чтобы вернуться");
                Console.WriteLine(_ramCounter.NextValue().ToString() + "MB");
                Console.WriteLine("{0}%", Track_Memory);
                for (int i = 0; i < 10; i++)
                {
                    if (i <= (Track_Memory / 10 - 1))
                    {
                        arr[i] = '|';
                    }
                    else
                    {
                        arr[i] = '-';
                    }
                    Console.Write(arr[i]);
                }

                Console.WriteLine();
                Track_Memory = (int)_ramPercent.NextValue();
                Thread.Sleep(1000);
                Console.Clear();
            }
            _Operations();
        }
    }
}