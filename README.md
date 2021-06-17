#include "stdafx.h"
#include <vector>
#include <iostream>
#include <windows.h>

using namespace std;
struct SDisk
{
   int size;
   int color;
};

void gotoxy( int x; int y);
{
    COORD p = {x, y};
    SetConsoleCursorPosition ( GetStdHandle( STD_OUTPUT_HANDLE ), p);
}

void setColor(int background, int text)
{
    HANDLE hStdOut = GetStdHandle (STD_OUTPUT_HANDLE);
    SetConsoleTextAttribute(hStdOut, (WORD)((background << 4) | text));
}

enum ConsoleColor
{
    BLACK          = 0,
    BLUE           = 1,
    GREEN          = 2,
    CYAN           = 3,
    RED            = 4,
    MAGENTA        = 5,
    BROWN          = 6,
    LIGHT_GRAY     = 7,
    DARKGRAY       = 8,
    LIGHT_BLUE     = 9,
    LIGHT_GREEN    = 10,
    LIGHT_CYAN     = 11,
    LIGHT_RED      = 12,
    LIGHT_MAGENTA  = 13,
    YELLOW         = 14,
    WHITE          = 15
};
void showTower(vector <SDisk> tower)
{
     for (int i = 0; i < tower.size(); i++)
    {
        cout << tower[i].size << "  ";
    }

     cout << endl
}

void showDisk(SDisk disk, int x, int y)
{
    gotoxy(x,y);
    setColor(WHITE,disk.color);
    for (int i = 0; i < disk.size(); i++)
    {
        cout << (char)219;
    }
    setColor(WHITE,BLACK);

}
void showTowers(vector < vector <SDisk> > towers, int px, int py, int sx)
{
    int x = px;
    int y = py;

    gotoxy(x,y);

    for (int i = 0; i < towers.size(); i++)
    {
        for (int j = 0; j < towers[i].size(); j++)
        {
            showDisk(towers[i][j],x,y);
            y--;
        }

           y = py;
           x += sx;

    }
}

class Game

{
    private:
        
             int N;
             vector < vector <SDisk> > towers;
             
             int x;
             int y;
             int sx;
     
    public:

    Game()
    {
         
         N = 5;
         
         x = 25;
         y = 10;
         sx = 11;

         vector <SDisk> tower0;
         vector <SDisk> tower1;
         vector <SDisk> tower2;

         SDisk disk;

         disk.size = 10;
         disk.color = BROWN;

         tower0.push_back(disk);
         tower1.push_back(disk);
         tower2.push_back(disk);

         for (int i = N; i >= 1; i--)
         {
         disk.size = i;
         disk.color = 1 + rand()%5;

         tower0.push_back(disk);
         }

         towers.push_back(towers[0]);
         towers.push_back(towers[1]);
         towers.push_back(towers[2]);
    
    }
    
         void showTowers()
      {
        int lx = x;
        int ly = y;

        gotoxy(lx,ly);

        for (int i = 0; i < towers.size(); i++)
       {
        for (int j = 0; j < towers[i].size(); j++)
        {
            showDisk(towers[i][j],lx,ly);
            ly--;
        }

           ly = y;
           lx += sx;

       }
      }
      
      bool shiftDisk(int sour, int dest)
      {
            SDisk disk0;
            SDisk disk1;
            
            
            sour--;
            dest--;


            disk0 = towers[sour][towers[sour].size()-1];
            disk1 = towers[dest][towers[dest].size()-1];


            if (disk0.size < disk1.size)
            {
                towers[sour].pop_back();
                towers[dest].push_back(disk0);
                return true;
            }
            else
            {
                return false;
            }
      }
      
      bool isWin()
      {
            if (towers[1].size() == N+1 || towers[2].size() == N+1)
            {
                return true;
            }
            else 
            {
                return false;
            }
      }

};


int _tmain(int argc, _TCHAR* argv[])
{
    //setlocale(LC_ALL, "rus");

    Game game;

    int sour = 0;
    int dest = 0;

    while(true)
{

            system("cls");
            game.showTowers();
            
            

            gotoxy(0,15);
            cout << "From ";
            cin >> sour;
            cout << "To ";
            cin >> dest;
            
            if (!game.shiftDisk(sour, dest))
            {
                cout << "Error!" << endl;
            }

            if (game.isWin())
            {
                system("cls");
                game.showTowers();
                gotoxy(0,20);
                cout << "Win!" << endl;
                break;
            }
}


    system("pause");

    return 0;
}
