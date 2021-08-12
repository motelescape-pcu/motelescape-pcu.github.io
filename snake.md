## C#을 이용한 Snake게임
# 메인 코드

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Diagnostics;

namespace ConsoleApplication17
{
    class Program
    {
        public static int[,] map = new int[17, 17] {
        {1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1},
        };
        public static Random rand = new Random();
        public static Stopwatch timer = new Stopwatch();
        public static Stopwatch scoretimer = new Stopwatch();
        public static char keyinfo;
        public static ConsoleKeyInfo key;
        public static bool isKeyPressed = false; //아직 필요없음
        public static int x = 8, y = 8, max = 1;
        public static int isItemDraw = 0;
        public static int[] tmpx = new int[224];
        public static int[] tmpy = new int[224];
        public static string str;
        public static long score = 0;


        static void Main(string[] args)
        {
            while (true)
            {
                tmpx[0] = 1;
                tmpy[0] = 1;
                timer.Start();
                scoretimer.Start();
                while (true)
                {
                    if (timer.ElapsedMilliseconds > 100)
                    {
                        drawbox();
                        input();
                        moving();
                        if (map[y, x] == 4 && max >= 5)
                        {
                            break;
                        }
                        map[y, x] = 2;
                        item();
                        timer.Restart();
                    }
                }
                Console.SetCursorPosition(12, 8);
                Console.Write("Game Over");
                Console.SetCursorPosition(10, 9);
                Console.Write("Restart? [Y/N]");
                str = Console.ReadLine();

                if (str == "Y" || str == "y")
                {
                    reset();
                }
                else break;
            }
        }
        



        public static void input() //키입력

        {
            if (Console.KeyAvailable)
            {
                key = Console.ReadKey();
                isKeyPressed = true;
            }
            if (Program.key.Key == ConsoleKey.UpArrow & isKeyPressed)
            {
                if (keyinfo != 'D')
                    keyinfo = 'U';
            }
            if (Program.key.Key == ConsoleKey.LeftArrow & isKeyPressed)
            {
                if (keyinfo != 'R')
                    keyinfo = 'L';
            }
            if (Program.key.Key == ConsoleKey.DownArrow & isKeyPressed)
            {
                if (keyinfo != 'U')
                    keyinfo = 'D';
            }
            if (Program.key.Key == ConsoleKey.RightArrow & isKeyPressed)
            {
                if (keyinfo != 'L')
                    keyinfo = 'R';
            }
        }




        public static void moving() //움직이기
        {
            tail();
            switch (keyinfo)
            {
                case 'U':
                    y--;
                    break;
                case 'D':
                    y++;
                    break;
                case 'L':
                    x--;
                    break;
                case 'R':
                    x++;
                    break;
            }
            switch (x)
            {
                case 0:
                    if (keyinfo == 'L')
                        x = 15;
                    break;
                case 16:
                    if (keyinfo == 'R')
                        x = 1;
                    break;
            }
            switch (y)
            {
                case 0:
                    if (keyinfo == 'U')
                        y = 15;
                    break;
                case 16:
                    if (keyinfo == 'D')
                        y = 1;
                    break;
            }
            if (map[y, x] == 3)
            {
                isItemDraw = 0;
                score += 1000-scoretimer.ElapsedMilliseconds/10;
                scoretimer.Restart();
            }
        }




        public static void drawbox()   //맵 그리기
        {
            Console.SetCursorPosition(0, 0);
            for (int i = 0; i <= 16; i++)
            {
                for (int j = 0; j <= 16; j++)
                {
                    Console.ForegroundColor = ConsoleColor.White;
                    if (map[i, j] == 0) Console.Out.Write("  ");
                    else if (map[i, j] == 1) Console.Out.Write("■");
                    else if (map[i, j] == 2) Console.Out.Write("◎");
                    else if (map[i, j] == 4) Console.Out.Write("○");
                    else if (map[i, j] == 3)
                    {
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        Console.Out.Write("★");
                    }
                    if (j == 16) Console.Out.WriteLine();
                }
            }
            Console.SetCursorPosition(40, 3);
            Console.Write("Score");
            Console.SetCursorPosition(40, 4);
            Console.Write(score);
        }





        public static void item() //아이템 출력
        {
            int ry = rand.Next(1, 16);
            int rx = rand.Next(1, 16);
            if (isItemDraw == 0 && map[ry, rx] != 2 && map[ry, rx] != 4)
            {
                map[ry, rx] = 3;
                isItemDraw = 1;
                max++;
            }
        }




        public static void tail()  //꼬리 출력
        {
            for (int i = max; i >= 1; i--)
            {
                tmpx[i] = tmpx[i - 1];
                tmpy[i] = tmpy[i - 1];
            }
            tmpx[0] = x;
            tmpy[0] = y;
            for (int i = 0; i < max; i++)
            {
                map[tmpy[i], tmpx[i]] = 4;
            }
            map[tmpy[max], tmpx[max]] = 0;
        }




        public static void reset()  //리셋
        {
            map = new int[17, 17] {
        {1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
        {1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1},
        };
            timer.Reset();
            isKeyPressed = false;
            x = 8;
            y = 8;
            max = 1;
            isItemDraw = 0;
            str = "N";
            keyinfo = 'F';
            tmpx = new int[224];
            tmpy = new int[224];
            score = 0;
        }
    }
}
