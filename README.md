# chess
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Chess
{public partial class Form1 : Form
    {
    public Form1()
        {
            InitializeComponent();
        }
    private struct cells //описываем структуру шахматной доски
        {   
          public  string name; //имя фигуры
           public  string BorW;// Цвет фигуры
           public int posX;//Координаты по Х
           public int posY;// Координаты по У
           public string pos_BorW;//Цвет клетки на которой стоит фигура
        };


    private static cells[,] pole; //выделяем память для структуры
    private static Point position; //тут будут находится координаты выбранной для хода фигуры
    private static bool stat=false; //переменная состояния
    private static string player="Белый";// если истина, то ходит белый


    private void fill_figurs(bool flag) //расставляем метки фиогур на поле
    { 
        if (flag) //если flag = true расставляем черные фигуры
        {
            pole[0, 0].name = "Ладья"; //Определение фигур
            pole[1, 0].name = "Конь";
            pole[2, 0].name = "Слон";
            pole[3, 0].name = "Ферзь";
            pole[4, 0].name = "Король";
            pole[5, 0].name = "Слон";
            pole[6, 0].name = "Конь";
            pole[7, 0].name = "Ладья";
            for(int i = 0; i < 8; i++)
            {
                pole[i, 1].name = "Пешка";
                pole[i, 1].BorW = "Черный"; //цвет
                pole[i, 0].BorW = "Черный";
                
            }
        }
        else//белые
        {
            pole[0, 7].name = "Ладья";
            pole[1, 7].name = "Конь";
            pole[2, 7].name = "Слон";
            pole[3, 7].name = "Ферзь";
            pole[4, 7].name = "Король";
            pole[5, 7].name = "Слон";   
            pole[6, 7].name = "Конь";
            pole[7, 7].name = "Ладья";
            for (int i = 0; i < 8; i++)
            {
                pole[i, 6].name = "Пешка";
                pole[i, 6].BorW = "Белый";
                pole[i, 7].BorW = "Белый";
            }
        }
    }

    private void start()//Предварительная разметка поля
    {
        pole = new cells[8, 8]; //pole[x,y] //Выделяем динамическую память для шахматной доски
        fill_figurs(true); //Вызываем функцию расстановки для черных фигур
        fill_figurs(false);// для белых фигур
        int count = 1; 
        int coordinataX = 0, coordinataY = 0; 
        for (int y = 0; y < 8; y++) //заполнение поля черными и белыми клетками
        {
            for (int x = 0; x < 8; x++)
            {
                if (count % 2 > 0)
                    pole[x, y].pos_BorW = "White";
                else
                    pole[x, y].pos_BorW = "Black"; 
                count++;
                pole[x, y].posX = coordinataX;
                pole[x, y].posY = coordinataY;
                coordinataX += 50;
            }
            count++;
            coordinataY += 50;
            coordinataX = 0; 
        }
    }

    private void drawing() //Функция которая будет  
    //загружать в память картинку с соответствующим изображением и
    //рисовать его на заданной графической области
    {
        Graphics board = this.panel1.CreateGraphics(); //обьявляем графическую область
        Bitmap white_check = new Bitmap(@"field\белое поле.bmp"); //загружаем картинки пустых клеток
        Bitmap black_check = new Bitmap(@"field\черное поле.bmp");
        Bitmap temp;//переменная для временного хранения загруженных картинок
        for (int y = 0; y < 8; y++)
            for (int x = 0; x < 8; x++)
                {
                    if (pole[x, y].pos_BorW == "White") //Определяем цвет клетки
                        board.DrawImage(white_check, pole[x, y].posX, pole[x, y].posY); //рисуем
                    else
                        board.DrawImage(black_check, pole[x, y].posX, pole[x, y].posY);
                  if (pole[x, y].name != null) //если поле name элемента не пустое
                    {
                        temp = new Bitmap(@"field\" + pole[x, y].BorW + pole[x, y].name + ".png");
                        board.DrawImage(temp, pole[x, y].posX, pole[x, y].posY);
                        temp.Dispose();//освобождаем временную переменную
                    }
                }
        white_check.Dispose(); //освобождаем занимаемую память
        black_check.Dispose();
        board.Dispose();
    } //draw()
        /* private void go(int x, int y) //Функция перемещения фигур по полю
     {
         if (pole[position.X, position.Y].BorW == player) //проверяем какой игрок сейчас ходит
         {                                                 //и фигуру какого цвета он хочет передвинуть
             if (pole[x, y].name != null) //если игрок хоче кого-то срубить
             {
                 if (pole[x, y].BorW != pole[position.X, position.Y].BorW)//проверяем не хочит ли игрок срубить свою фигуру
                 {
                         
                     pole[x, y].name = pole[position.X, position.Y].name; //передвигаем фигуру
                     pole[x, y].BorW = pole[position.X, position.Y].BorW;
                     pole[position.X, position.Y].name = null; //зануляем больше ненужные поля
                     pole[position.X, position.Y].BorW = null;
                 }
             }
             else //если игрок просто ходит
             {
                 pole[x, y].name = pole[position.X, position.Y].name; //передвигаем фигуру на пустое место
                 pole[x, y].BorW = pole[position.X, position.Y].BorW;
                 pole[position.X, position.Y].name = null;
                 pole[position.X, position.Y].BorW = null;
             }
         }*/
        private void button1_Click(object sender, EventArgs e)
        {
            start();//Заполняем шахматную доску
            drawing();//Выводим поле
            //go();
        }
    }
}
