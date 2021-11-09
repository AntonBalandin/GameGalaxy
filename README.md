# GameGalaxy
Galaxy game project
#include<iostream>
#include<iomanip>
#include<Windows.h>
#include<ctime>
#include<cmath>
#include<string>
#include<cstdlib>
#include<conio.h>
using namespace std;

bool GameOver = false;

const int row = 12; //15- для работы в аудитории.(12)
const int col = 26; //39 (26)
int i, j;
char mas[row][col];
extern int x_1, y_1;
extern int x_2, y_2;
int x_1 = col / 2, y_1 = row - 2;
int x_2 = x_1, y_2 = y_1 - 1;
short x_3 = 1, y_3 = 0; // это стартовый корабль врага
short step = 5;
extern bool bullet_stop;
bool bullet_stop = true;
int a;
enum D { Stop = 0, Up };
D dir;

void OutputField(HANDLE& hd, COORD& cd)
{
	extern int x_1, y_1;
	extern int x_2, y_2;

	system("cls");

	for (int i = 0; i < row; i++)
	{
		for (int j = 0; j < col; j++)
		{
			if (i == y_3 && j == x_3 ||
				i == y_3 && j == x_3 + 1 ||

				i == y_3 && j == x_3 + step ||
				i == y_3 && j == x_3 + step + 1 ||

				i == y_3 && j == x_3 + 2 * step ||
				i == y_3 && j == x_3 + 2 * step + 1 ||

				i == y_3 && j == x_3 + 3 * step ||
				i == y_3 && j == x_3 + 3 * step + 1 ||

				i == y_3 && j == x_3 + 4 * step ||
				i == y_3 && j == x_3 + 4 * step + 1 ||

				i == y_3 && j == x_3 + 5 * step ||
				i == y_3 && j == x_3 + 5 * step + 1 ||

				i == y_3 && j == x_3 + 6 * step ||
				i == y_3 && j == x_3 + 6 * step + 1 ||

				i == y_3 && j == x_3 + 7 * step ||
				i == y_3 && j == x_3 + 7 * step + 1 ||

				i == y_3 && j == x_3 + 9 * step ||
				i == y_3 && j == x_3 + 9 * step + 1)

				mas[i][j] = '3';



			else if (i == y_2 && j == x_2)
				mas[i][j] = '2';

			else if (i == y_1 && j == x_1)
				mas[i][j] = '1';
			else
			{
				mas[i][j] = ' ';
			}
			cout << setw(3) << mas[i][j];
		}

		if (i != 14)
			cout << "\n\n";
	}

}

void Shooting(int& x, int& y) // стрельба
{
	extern bool bullet_stop;
	bullet_stop = false;
}

void  Moving() // передвижение стрелка
{

	if (_kbhit())
	{
		extern int x_1, x_2, y_1, y_2;

		int a = _getch();

		if (a == 27) exit(0); // выход из игры
		if (a == 32) // нажатие на пробел
		{
			//вычисляю х2 ,у2
			x_2 = x_1;
			y_2 = y_1 - 1;
			Shooting(x_2, y_2);

		}

		if (a == 0 || a == 224)
		{

			a = _getch();
			switch (a)
			{
			case 72:
				if (y_1 > 5) //8 (5)  передвижение стрелка вверх
				{
					y_1--;
				} break;

			case 75:
				if (x_1 > 0) // влево
				{
					x_1--;
				} break;
			case 80:
				if (y_1 < 11) //14 (11)  вниз
				{
					y_1++;
				} break;
			case 77:
				if (x_1 < 25) //38 (25)  вправо
				{
					x_1++;
				} break;
			}
		}
		dir = Stop;
		if (a == 32)
		{
			dir = Up;
		}
	}
}
void BulletMovement() // полёт пули
{
	switch (dir)
	{
	case Up:
		if (y_2 >= 0)
		{
			y_2--;
			break;
		}
	}
}

int main()
{
	setlocale(LC_ALL, "RUS");

	HANDLE hd = GetStdHandle(STD_OUTPUT_HANDLE);
	COORD cd;

	extern bool GameOver;
	extern int y_2;
	extern bool bullet_stop;
	while (!GameOver)
	{
		OutputField(hd, cd);
		Moving();
		BulletMovement();

	}
	return 0;
}
