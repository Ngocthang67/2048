# 2048
#include <iostream>
#include <cstdio>
#include <time.h>
#include <Windows.h>
#include <conio.h>
#include <fstream>
#include <string>

#define consoleHeight 30
#define consoleWidth 100
#define gameboardWidth 24
#define gameboardHeight 16

using namespace std;

int ConSo[4][4] = { 0 };
int DiemSo;
bool replayGame = false;
char KT=219;

// con tro
void contro(bool CursorVisibility)
{
	HANDLE handle = GetStdHandle(STD_OUTPUT_HANDLE);
	CONSOLE_CURSOR_INFO cursor = { 1, CursorVisibility };
	SetConsoleCursorInfo(handle, &cursor);
}

//xóa màn hình
void clrscr()
{
	CONSOLE_SCREEN_BUFFER_INFO	csbiInfo;
	HANDLE	hConsoleOut;
	COORD	Home = { 0, 0 };
	DWORD	dummy;

	hConsoleOut = GetStdHandle(STD_OUTPUT_HANDLE);
	GetConsoleScreenBufferInfo(hConsoleOut, &csbiInfo);

	FillConsoleOutputCharacter(hConsoleOut, ' ', csbiInfo.dwSize.X * csbiInfo.dwSize.Y, Home, &dummy);
	csbiInfo.dwCursorPosition.X = 0;
	csbiInfo.dwCursorPosition.Y = 0;
	SetConsoleCursorPosition(hConsoleOut, csbiInfo.dwCursorPosition);
}


// toa do x,y
void gotoXY(int x, int y)
{	
	HANDLE hConsoleOutput;    
	COORD Cursor_an_Pos = {x-1,y-1};   
	hConsoleOutput = GetStdHandle(STD_OUTPUT_HANDLE);    
	SetConsoleCursorPosition(hConsoleOutput , Cursor_an_Pos);
	
}



// Thay doi mau text
void TextColor(int color)
{
	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

//info games 
void infogames()
{
	TextColor(11);
	cout<<endl<<endl<<endl;
	TextColor(14);
	cout<<" 222222222222222          000000000             444444444        888888888     "<<endl; Sleep(60);
	cout<<"2:::::::::::::::22      00:::::::::00          4::::::::4      88:::::::::88   "<<endl; Sleep(70);
	cout<<"2::::::222222:::::2   00:::::::::::::00       4:::::::::4    88:::::::::::::88 "<<endl; Sleep(80);
	cout<<"2222222     2:::::2  0:::::::000:::::::0     4::::44::::4   8::::::88888::::::8"<<endl; Sleep(90);
	cout<<"            2:::::2  0::::::0   0::::::0    4::::4 4::::4   8:::::8     8:::::8"<<endl; Sleep(100);
	cout<<"            2:::::2  0:::::0     0:::::0   4::::4  4::::4   8:::::8     8:::::8"<<endl; Sleep(110);
	cout<<"         2222::::2   0:::::0     0:::::0  4::::4   4::::4    8:::::88888:::::8 "<<endl; Sleep(120);
	cout<<"    22222::::::22    0:::::0 000 0:::::0 4::::444444::::444   8:::::::::::::8  "<<endl; Sleep(130);
	cout<<"  22::::::::222      0:::::0 000 0:::::0 4::::::::::::::::4  8:::::88888:::::8 "<<endl; Sleep(140);
	cout<<" 2:::::22222         0:::::0     0:::::0 4444444444:::::444 8:::::8     8:::::8"<<endl; Sleep(150);
	cout<<"2:::::2              0:::::0     0:::::0           4::::4   8:::::8     8:::::8"<<endl; Sleep(160);
	cout<<"2:::::2              0::::::0   0::::::0           4::::4   8:::::8     8:::::8"<<endl; Sleep(170);
	cout<<"2:::::2       222222 0:::::::000:::::::0           4::::4   8::::::88888::::::8"<<endl; Sleep(180);
	cout<<"2::::::2222222:::::2  00:::::::::::::00          44::::::44  88:::::::::::::88 "<<endl; Sleep(190);
	cout<<"2::::::::::::::::::2    00:::::::::00            4::::::::4    88:::::::::88   "<<endl; Sleep(200);
	cout<<"22222222222222222222      000000000              4444444444      888888888     "<<endl; Sleep(210);
	cout<<endl; Sleep(50);
	cout<<endl; Sleep(50);
	TextColor(14);
	gotoXY(50, 24);
	cout<<"(c)";Sleep(50);
	cout<<"N";Sleep(30);
	cout<<"g";Sleep(40);
	cout<<"o";Sleep(50);
	cout<<"c";Sleep(60);
	cout<<"T";Sleep(70);
	cout<<"h";Sleep(80);
	cout<<"a";Sleep(90);
	cout<<"n";Sleep(100);
	cout<<"g";Sleep(110);
	cout<<" - ";Sleep(120);
	cout<<"v";Sleep(130);
	cout<<"e";Sleep(140);
	cout<<"r ";Sleep(150);
	cout<<"2";Sleep(160);
	cout<<"0";Sleep(170);
	cout<<"1";Sleep(180);
	cout<<"8";Sleep(190);
	
	gotoXY(2, 22);
	contro(true);
	TextColor(13);
	cout<<"P";Sleep(50);cout<<"r";Sleep(60);cout<<"e";Sleep(70);
	cout<<"s";Sleep(80);cout<<"s";Sleep(90);cout<<" a";Sleep(100);
	cout<<"n";Sleep(110);cout<<"y";Sleep(120);cout<<" k";Sleep(130);
	cout<<"e";Sleep(140);cout<<"y";Sleep(150);cout<<" t";Sleep(160);
	cout<<"o";Sleep(170);cout<<" s";Sleep(180);cout<<"t";Sleep(190);
	cout<<"a";Sleep(200);cout<<"r";Sleep(210);cout<<"t";Sleep(220);
	cout<<" g";Sleep(220);cout<<"a";Sleep(220);cout<<"m";Sleep(220);
	cout<<"e";Sleep(220);cout<<".";Sleep(220);cout<<".";Sleep(220);
	cout<<". ";Sleep(220);
	

	_getch();
}
//ve khung game
void drawFrame()
{
	for (int i = 0; i <= gameboardWidth; i++) {
		for (int j = 0; j <= gameboardHeight; j++) {
			if (j % 4 == 0) {
				gotoXY(i + 1, j + 1);
				TextColor(14);
				cout<<KT;
			}

			else if (i % 6 == 0) {
				gotoXY(i + 1, j + 1);
				TextColor(14);
				cout<<KT;
			}

			else {
				TextColor(3);
				gotoXY(i + 1, j + 1);
				cout<<KT;
			}
		}
	}
}

//bat dau tro choi
void playgame()
{
	clrscr();
	drawFrame();

	DiemSo = 0;
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			ConSo[i][j] = 0;
		}
	}
	srand(time(NULL));
	int newTile[4];

	newTile[0] = rand() % 4;
	newTile[1] = rand() % 4;

	do {
		newTile[2] = rand() % 4;
		newTile[3] = rand() % 4;
	} while (newTile[0] == newTile[2] || newTile[1] == newTile[3]);

	ConSo[newTile[0]][newTile[1]] = 2;
	ConSo[newTile[2]][newTile[3]] = 2;
}



void Lentren()
{
	for (int j = 0; j < 4; j++) {
		int t = 0;
		for (int i = 0; i < 4; i++) {
			if (ConSo[i][j] != 0) {
				ConSo[t][j] = ConSo[i][j];
				t++;
			}
		}
		for (int i = t; i < 4; i++) ConSo[i][j] = 0;
	}
	for (int j = 0; j < 4; j++) {
		int t = 0;
		for (int i = 0; i < 4; i++) {
			if (ConSo != 0) {
				if (ConSo[i][j] == ConSo[i + 1][j]) {
					ConSo[t][j] = 2 * ConSo[i][j];
					DiemSo += ConSo[t][j];
					t++;
					i++;
				}
				else {
					ConSo[t][j] = ConSo[i][j];
					t++;
				}
			}
		}
		for (int i = t; i < 4; i++) ConSo[i][j] = 0;
	}

}

void Xuongduoi()
{
	for (int j = 0; j < 4; j++) {
		int t = 3;
		for (int i = 3; i >= 0; i--) {
			if (ConSo[i][j] != 0) {
				ConSo[t][j] = ConSo[i][j];
				t--;
			}
		}
		for (int i = t; i >= 0; i--) ConSo[i][j] = 0;
	}
	for (int j = 0; j < 4; j++) {
		int t = 3;
		for (int i = 3; i >= 0; i--) {
			if (ConSo != 0) {
				if (ConSo[i][j] == ConSo[i - 1][j]) {
					ConSo[t][j] = 2 * ConSo[i][j];
					DiemSo += ConSo[t][j];
					t--;
					i--;
				}
				else {
					ConSo[t][j] = ConSo[i][j];
					t--;
				}
			}
		}
		for (int i = t; i >= 0; i--) ConSo[i][j] = 0;
	}

}

void Bentrai()
{
	for (int i = 0; i < 4; i++) {
		int t = 0;
		for (int j = 0; j < 4; j++) {
			if (ConSo[i][j] != 0) {
				ConSo[i][t] = ConSo[i][j];
				t++;
			}
		}
		for (int j = t; j < 4; j++) ConSo[i][j] = 0;
	}
	for (int i = 0; i < 4; i++) {
		int t = 0;
		for (int j = 0; j < 4; j++) {
			if (ConSo[i][j] != 0) {
				if (ConSo[i][j] == ConSo[i][j + 1]) {
					ConSo[i][t] = 2 * ConSo[i][j];
					DiemSo += ConSo[i][t];
					j++;
					t++;
				}
				else {
					ConSo[i][t] = ConSo[i][j];
					t++;
				}
			}
		}
		for (int j = t; j < 4; j++) ConSo[i][j] = 0;
	}

}

void Benphai()
{
	for (int i = 0; i < 4; i++) {
		int t = 3;
		for (int j = 3; j >= 0; j--) {
			if (ConSo[i][j] != 0) {
				ConSo[i][t] = ConSo[i][j];
				t--;
			}
		}
		for (int j = t; j >= 0; j--) ConSo[i][j] = 0;
	}
	for (int i = 0; i < 4; i++) {
		int t = 3;
		for (int j = 3; j >= 0; j--) {
			if (ConSo[i][j] != 0) {
				if (ConSo[i][j] == ConSo[i][j - 1]) {
					ConSo[i][t] = 2 * ConSo[i][j];
					DiemSo += ConSo[i][t];
					j--;
					t--;
				}
				else {
					ConSo[i][t] = ConSo[i][j];
					t--;
				}
			}
		}
		for (int j = t; j >= 0; j--) ConSo[i][j] = 0;
	}
}
//mau cua cac con so
void setColor(int value)
{
	switch (value) {
	case 2:		TextColor(48); break;
	case 4:		TextColor(49); break;
	case 8:		TextColor(50); break;
	case 16:	TextColor(52); break;
	case 32:	TextColor(53); break;
	case 64:	TextColor(54); break;
	case 128:	TextColor(55); break;
	case 256:	TextColor(56); break;
	case 512:	TextColor(57); break;
	case 1024:  TextColor(58); break;
	case 2048:  TextColor(59); break;
	case 4096:  TextColor(60); break;
	case 8192:  TextColor(61); break;
	case 16384: TextColor(62); break;
	}
}
// Hien thi

void showConSo()
{
	
	gotoXY(35, 7);
	TextColor(15);
	cout<<"Intruction: ";
	gotoXY(35, 8);
	TextColor(10);
	cout<<"A: Turn Left";
	gotoXY(35, 9);
	TextColor(9);
	cout<<"S: Go Down";
	gotoXY(35, 10);
	TextColor(14);
	cout<<"D: Turn Right";
	gotoXY(35, 11);
	TextColor(13);
	cout<<"W: Go Up";
	gotoXY(35, 13);
	TextColor(15);
	cout<<"Press 'r' to replay, 'x' to exit!";

	for (int i = 3; i <= 21; i += 6) {
		for (int j = 3; j <= 15; j += 4) {
			TextColor(51);
			gotoXY(i, j); 
			cout<<KT<<KT<<KT<<KT;
			setColor(ConSo[(j - 3) / 4][(i - 3) / 6]);
			if (ConSo[(j - 3) / 4][(i - 3) / 6] == 0) {
				TextColor(51);
				gotoXY(i, j); 
				cout<<KT<<KT<<KT<<KT;;
			}
			else if (ConSo[(j - 3) / 4][(i - 3) / 6] < 100) {
				gotoXY(i + 1, j); 
				cout<<ConSo[(j - 3) / 4][(i - 3) / 6];
			}
			else {
				gotoXY(i, j);
				cout<< ConSo[(j - 3) / 4][(i - 3) / 6];
			}
			//gotoXY(i + 6, j + 6); cout<< i<<j;
		}
	}
	gotoXY(35, 3);
	TextColor(11);
	cout<<"Score: ";
	TextColor(12);
	cout<<DiemSo;
}

// kiem tra cac ô trong
bool checkbox()
{
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			if (ConSo[i][j] == 0) {
				return true;
			}
		}
	}
	return false;
}

// Tao so

void addTile()
{
	if (checkbox() == false) return;
	int x, y;
	srand(time(NULL));
	do {
		x = rand() % 4;
		y = rand() % 4;
	} while (ConSo[x][y] != 0);

	
	
	//Thay do kho trong game
	int s = rand() % 100;
	if(DiemSo<1000){
		if (s > 80) ConSo[x][y] = 4;
		else ConSo[x][y] = 2;
	}else if(DiemSo<3000){
		if (s > 70) ConSo[x][y] = 4;
		else ConSo[x][y] = 2;
	}
	else if(DiemSo<5000){
		if (s > 60) ConSo[x][y] = 4;
		else ConSo[x][y] = 2;
	}else if(DiemSo>=5000){
		if (s > 50) ConSo[x][y] = 4;
		else ConSo[x][y] = 2;
	}
	
}

int boardCheck[4][4];

void creCheck()
{
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			boardCheck[i][j] = ConSo[i][j];
		}
	}
}

bool checkMove()
{
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			if (boardCheck[i][j] != ConSo[i][j]) return true;
		}
	}
	return false;
}

bool checkGameOver()
{
	if (checkbox() == false) {
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				if (ConSo[i][j] == ConSo[i][j + 1] || 
					ConSo[i][j] == ConSo[i + 1][j]) return false;
			}
		}
	}
	else if (checkbox() == true) return false;
	return true;
}



int main()
{
	infogames();
	playgame();

	while (true) {

		if (replayGame == true) {
			playgame();
			replayGame = false;
		}
		contro(false);
		showConSo();

		if (checkGameOver() == true) {
			gotoXY(35, 16);
			cout<<"GAME OVER!";
		}

		char key = _getch();
		creCheck();

		if (key == 'w' || key == 'W') {
			Lentren();
		}
		else if (key == 's' || key == 'S') {
			Xuongduoi();
		}
		else if (key == 'a' || key == 'A') {
			Bentrai();
		}
		else if (key == 'd' || key == 'D') {
			Benphai();
		}
		else if (key == 'r' || key == 'R') {
			replayGame = true;
		}
		else if (key == 'x' || key == 'X') {
			exit(true);
		}
		else continue;

		if (checkMove() == false) continue;
		addTile();
	}

	_getch();

	return 0;
}
