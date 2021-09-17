

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Типо_Paint
{
    public partial class Form1 : Form
    {

        Pen blackpen;
        int click;

        public Form1()
        {

            InitializeComponent();
            pictureBox1.Width = 750;
            pictureBox1.Height = 750;
            pictureBox1.BackColor = Color.White;
            pictureBox2.BackColor = Color.Black;
            label5.Text = "Черный";
            blackpen = new Pen(Color.Black, trackBar1.Value);
            SetSize();
            checkedListBox1.SetItemChecked(8, true);

        }
        private bool ismouse = false;
        Graphics g;
        private class ArrayPoints
        {
            private int index = 0;
            private Point[] points;

            public ArrayPoints(int size)
            {
                // if (size <= 0) { size = i; }
                points = new Point[size];
            }

            public void SetPoint(int x, int y)
            {
                if (index >= points.Length)
                {
                    index = 0;
                }
                points[index] = new Point(x, y);
                index++;
            }
            public void ResetPoints()
            {
                index = 0;
            }
            public int GetCountPoints()
            {
                return index;
            }
            public Point[] GetPoints()
            {
                return points;
            }

        }
        Bitmap map = new Bitmap(100, 100);
        private ArrayPoints arrayPoints = new ArrayPoints(2);
        private ArrayPoints arrayPointsTriangle = new ArrayPoints(3);
        private void SetSize()
        {
            Rectangle rectangle = Screen.PrimaryScreen.Bounds;
            map = new Bitmap(rectangle.Width, rectangle.Height);

            g = Graphics.FromImage(map);
            blackpen.StartCap = System.Drawing.Drawing2D.LineCap.Round;
            blackpen.EndCap = System.Drawing.Drawing2D.LineCap.Round;
        }
        private void pictureBox1_MouseMove(object sender, MouseEventArgs e)
        {
            switch (click)
            {
                case 1:
                    if (e.Button != MouseButtons.Left)
                    {
                        return;
                    }
                    arrayPoints.SetPoint(e.X, e.Y);
                    if (arrayPoints.GetCountPoints() >= 2)
                    {

                        g.DrawLines(blackpen, arrayPoints.GetPoints());
                        pictureBox1.Image = map;
                        arrayPoints.SetPoint(e.X, e.Y);
                        g = Graphics.FromImage(map);
                    }
                    break;


            }
        }
        private class ArrayPointsRectangle
        {
            private int index;
            private int[] point;
            
            public ArrayPointsRectangle(int size)
            {
                point = new int[size+2];
            }
            public void SetPoint(int x ,int y)
            {
                point[index] = x;
                index++;
                point[index] = y;
                index++;
            }
            
            public int CheckMinus()
            {
        
                if (point[2] - point[0] < 0 && point[3] - point[1] < 0)
                {
                    return 1;
                }
                else if(point[2] - point[0] < 0 && point[3] - point[1] > 0)
                {
                    return 2;
                }
                else if (point[2] - point[0] > 0 && point[3] - point[1] > 0)
                {
                    return 3;
                }
                else return 0;
            }
            public int Width()
            {
                if (point[2] - point[0] < 0)
                {
                    point[2] = point[2] + point[0];
                    point[0] = point[2] - point[0];
                    point[2] = point[2] -point[0];
                }
                return point[2] - point[0];
            }
            public int Heigth()
            {
                if (point[3] - point[1] < 0)
                {
                    point[3] = point[3] + point[1];
                    point[1] = point[3] - point[1];
                    point[3] = point[3] - point[1];
                }
            
                return point[3] - point[1];
            }
         
            public int GetPoints(bool k)
            {
                if (k == true) {
                    return point[0];
                }
                else return point[1];
            }
            public int GetCountPoints()
            {
                return index;
            }
            public void ResetPoints()
            {
                index = 0;
            }
        }
        private ArrayPointsRectangle arrayPointsRectangle=new ArrayPointsRectangle(2);
        private void pictureBox1_MouseDown(object sender, MouseEventArgs e)
        {
            switch (click) {
                case 2:
                    arrayPointsRectangle.SetPoint(e.X, e.Y);
                    if (arrayPointsRectangle.GetCountPoints() >= 4)
                    {
                        if (arrayPointsRectangle.CheckMinus() == 3)
                        {
                            g.DrawRectangle(blackpen, arrayPointsRectangle.GetPoints(true),
                                arrayPointsRectangle.GetPoints(false), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        else if (arrayPointsRectangle.CheckMinus() == 2)
                        {
                            g.DrawRectangle(blackpen, arrayPointsRectangle.GetPoints(true)- arrayPointsRectangle.Width(), 
                                arrayPointsRectangle.GetPoints(false), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }   
                        else if (arrayPointsRectangle.CheckMinus() == 1)
                        {
                            g.DrawRectangle(blackpen, arrayPointsRectangle.GetPoints(true) - arrayPointsRectangle.Width(),
                                arrayPointsRectangle.GetPoints(false)-arrayPointsRectangle.Heigth(), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        else
                        {
                            g.DrawRectangle(blackpen, arrayPointsRectangle.GetPoints(true),
                                arrayPointsRectangle.GetPoints(false) - arrayPointsRectangle.Heigth(), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        pictureBox1.Image = map;
        
                        g = Graphics.FromImage(map);
                        arrayPointsRectangle.ResetPoints();
                    }
                    break;
                case 3:
                    arrayPointsRectangle.SetPoint(e.X, e.Y);
                    if (arrayPointsRectangle.GetCountPoints() >= 4)
                    {
                        if (arrayPointsRectangle.CheckMinus() == 3)
                        {
                            g.DrawEllipse(blackpen, arrayPointsRectangle.GetPoints(true),
                                arrayPointsRectangle.GetPoints(false), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        else if (arrayPointsRectangle.CheckMinus() == 2)
                        {
                            g.DrawEllipse(blackpen, arrayPointsRectangle.GetPoints(true) - arrayPointsRectangle.Width(),
                                arrayPointsRectangle.GetPoints(false), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        else if (arrayPointsRectangle.CheckMinus() == 1)
                        {
                            g.DrawEllipse(blackpen, arrayPointsRectangle.GetPoints(true) - arrayPointsRectangle.Width(),
                                arrayPointsRectangle.GetPoints(false) - arrayPointsRectangle.Heigth(), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        else
                        {
                            g.DrawEllipse(blackpen, arrayPointsRectangle.GetPoints(true),
                                arrayPointsRectangle.GetPoints(false) - arrayPointsRectangle.Heigth(), arrayPointsRectangle.Width(), arrayPointsRectangle.Heigth());
                        }
                        pictureBox1.Image = map;
                        g = Graphics.FromImage(map);
                        arrayPointsRectangle.ResetPoints();
                    }
                    break;
                case 4:
                    arrayPointsTriangle.SetPoint(e.X, e.Y);
                    if (arrayPointsTriangle.GetCountPoints() >= 3)
                    {

                        g.DrawPolygon(blackpen, arrayPointsTriangle.GetPoints());
                        pictureBox1.Image = map;
                        arrayPointsTriangle.SetPoint(e.X, e.Y);
                        g = Graphics.FromImage(map);
                        arrayPointsTriangle.ResetPoints();
                    }
                    break;


            }
        }

        private void pictureBox1_MouseUp(object sender, MouseEventArgs e)
        {
            ismouse = false;
            if (click == 1)
            {
                arrayPoints.ResetPoints();
            }
        }
        private void сохранитьToolStripMenuItem_Click(object sender, EventArgs e)
        {


            if (pictureBox1.Image != null) //если в pictureBox есть изображение
            {
                //создание диалогового окна "Сохранить как..", для сохранения изображения
                SaveFileDialog savedialog = new SaveFileDialog();
                savedialog.Title = "Сохранить картинку как...";
                //отображать ли предупреждение, если пользователь указывает имя уже существующего файла
                savedialog.OverwritePrompt = true;
                //отображать ли предупреждение, если пользователь указывает несуществующий путь
                savedialog.CheckPathExists = true;
                //список форматов файла, отображаемый в поле "Тип файла"
                savedialog.Filter = "Image Files(*.BMP)|*.BMP|Image Files(*.JPG)|*.JPG|Image Files(*.PNG)|*.PNG|All files (*.*)|*.*";
                //отображается ли кнопка "Справка" в диалоговом окне
                savedialog.ShowHelp = true;
                if (savedialog.ShowDialog() == DialogResult.OK) //если в диалоговом окне нажата кнопка "ОК"
                {
                    try
                    {
                        pictureBox1.Image.Save(savedialog.FileName);
                    }
                    catch
                    {
                        MessageBox.Show("Невозможно сохранить изображение", "Ошибка",
                        MessageBoxButtons.OK, MessageBoxIcon.Error);
                    }
                }
            }
        }

        private void открытьToolStripMenuItem_Click(object sender, EventArgs e)
        {
           
                OpenFileDialog dialog = new OpenFileDialog();
                dialog.Filter = "Image files (*.BMP, *.JPG,  *.TIF, *.PNG)|*.bmp;*.jpg; *.png;";
                if (dialog.ShowDialog() == DialogResult.OK)
                {
                    Image image = Image.FromFile(dialog.FileName);
                  
                    pictureBox1.Width = 750;
                    pictureBox1.Height =750;
                    map = new Bitmap(Image.FromFile(dialog.FileName), 750, 750);
                    pictureBox1.Image = new Bitmap(map);
                }
            
        }

        

        private void radioButton1_CheckedChanged(object sender, EventArgs e)
        {
            if (radioButton1.Checked == true)
            {
                click = 1;
                radioButton2.Checked = false;
                radioButton3.Checked = false;
                radioButton4.Checked = false;
            }
        }

        private void radioButton2_CheckedChanged(object sender, EventArgs e)
        {
            if (radioButton2.Checked == true)
            {
                click = 2;
                radioButton1.Checked = false;
                radioButton3.Checked = false;
                radioButton4.Checked = false;
            }
            
            
        }

 

       

        private void trackBar1_Scroll(object sender, EventArgs e)
        {
            blackpen.Width = trackBar1.Value;
        }

        private void radioButton3_CheckedChanged(object sender, EventArgs e)
        {
            if (radioButton3.Checked == true)
            {
                click = 3;
                radioButton2.Checked = false;
                radioButton1.Checked = false;
                radioButton4.Checked = false;
            }
        }

        private void button1_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }

        private void button2_Click(object sender, EventArgs e)
        {

            pictureBox1.Image = null;
            map = null;
            SetSize();
        }

        private void radioButton4_CheckedChanged(object sender, EventArgs e)
        {
            if (radioButton4.Checked == true)
            {
                click = 4;
                radioButton2.Checked = false;
                radioButton3.Checked = false;
                radioButton1.Checked = false;
            }
        }
        private void checkedListBox1_SelectedIndexChanged(object sender, EventArgs e)

        {
            int index = 0;
            pictureBox2.Visible = true;
            label5.Visible = true;
            checkedListBox1.CheckOnClick = true;
            index = checkedListBox1.SelectedIndex;
            void check()
            {
                if (checkedListBox1.CheckedItems.Count > 1)
                {
                    for (int i = 0; i < checkedListBox1.Items.Count; i++)
                        checkedListBox1.SetItemChecked(i, false);
                    checkedListBox1.SetItemChecked(checkedListBox1.SelectedIndex, true);
                }
            }
            switch (index)
            {
                case 0:
                    pictureBox2.BackColor = Color.White;
                    label5.Text = "Белый";
                    blackpen.Color = Color.White;
                    check();
                    break;
                case 1:
                    blackpen.Color = Color.Cyan;
                    pictureBox2.BackColor = Color.Cyan;
                    label5.Text = "Голубой";
                    check();
                    break;
                case 2:
                    blackpen.Color = Color.Yellow;
                    pictureBox2.BackColor = Color.Yellow;
                    label5.Text = "Желтый";
                    check();
                    break;
                case 3:
                    blackpen.Color = Color.Green;
                    pictureBox2.BackColor = Color.Green;
                    label5.Text = "Зеленый";
                    check();
                    break;
                case 4:
                    blackpen.Color = Color.Red;
                    pictureBox2.BackColor = Color.Red;
                    label5.Text = "Красный";
                    check();
                    break;
                case 5:
                    blackpen.Color = Color.Orange;
                    pictureBox2.BackColor = Color.Orange;
                    label5.Text = "Оранжевый";
                    check();
                    break;
                case 6:
                    blackpen.Color = Color.Blue;
                    pictureBox2.BackColor = Color.Blue;
                    label5.Text = "Синий";
                    check();
                    break;
                case 7:
                    blackpen.Color = Color.Purple;
                    pictureBox2.BackColor = Color.Purple;
                    label5.Text = "Фиолетовый";
                    check();
                    break;
                case 8:
                    blackpen.Color = Color.Black;
                    pictureBox2.BackColor = Color.Black;
                    label5.Text = "Черный";
                    check();
                    break;
            }
        }

       
    }
 
    }
