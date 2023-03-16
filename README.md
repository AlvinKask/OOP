# OOP

\\tptlive\avalik\B307\AN_B307

[AK_esimene_projekt.zip](https://github.com/AlvinKask/OOP/files/10746697/AK_esimene_projekt.zip)


02.02.2023 tunnis tehtud sodi
[02.02.2023.zip](https://github.com/AlvinKask/OOP/files/10567249/02.02.2023.zip)

[16.02.2023.zip](https://github.com/AlvinKask/OOP/files/10754401/16.02.2023.zip)




- [OOP_raamat.pdf](https://github.com/AlvinKask/OOP/files/10565809/OOP_raamat.pdf)
- [Progr_alused.pdf](https://github.com/AlvinKask/OOP/files/10565910/Progr_alused.pdf)

Windows Forms Type
View - Solutions Explorer - Rename Form

Label - autosize = false
Checkbox - checked = true

- http://www.csharphelper.com/


- [02.03.2023.zip](https://github.com/AlvinKask/OOP/files/10869761/02.03.2023.zip)

- [16.03.2023.zip](https://github.com/AlvinKask/OOP/files/10989219/16.03.2023.zip)

```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace AK_WindowsFormsApp1
{
    public partial class AK_Form3 : Form
    {
        string failinimi;
        int w, h;
        int x1, y1, x2, y2;
        Image pic = null;

        public AK_Form3()
        {
            InitializeComponent();
            w = AK_pic1.Width;
            h = AK_pic1.Height;
        }

        private void AK_open_Click(object sender, EventArgs e)
        {
            string t = "JPG failid|*.jpg";
            t = t + "|PNG failid|*.png";
            t = t + "|JPG ja PNG failid|*.jpg;*.png";
            t = t + "|Kõik failid|*.*";
            // AK_groupBox1.Text = t;
            AK_openFileDialog1.Filter = t;
            AK_openFileDialog1.FileName = "";
            AK_openFileDialog1.ShowDialog();
            failinimi = AK_openFileDialog1.FileName;
            if (failinimi == "") return;
            // AK_groupBox1.Text = failinimi;
            AK_pic1.Image = Image.FromFile(failinimi);
            pic = Image.FromFile(failinimi);
            AK_saveas.Enabled = true;
        }

        private void AK_saveas_Click(object sender, EventArgs e)
        {
            AK_saveFileDialog1.Filter = "JPG failid|*.jpg";
            AK_saveFileDialog1.FileName = "";
            AK_saveFileDialog1.ShowDialog();
            failinimi = AK_saveFileDialog1.FileName;
            if (failinimi == "") return;
            if (AK_pic2.Image == null) return;
            Bitmap b = new Bitmap(AK_pic2.Image);
            b.Save(failinimi, System.Drawing.Imaging.ImageFormat.Jpeg);
        }

        private void AK_radioButton1_CheckedChanged(object sender, EventArgs e)
        {
            AK_pic1.Width = w;
            AK_pic1.Height = h;
            AK_pic1.SizeMode = PictureBoxSizeMode.Normal;
        }

        private void AK_radioButton2_CheckedChanged(object sender, EventArgs e)
        {
            AK_pic1.Width = w;
            AK_pic1.Height = h;
            AK_pic1.SizeMode = PictureBoxSizeMode.StretchImage;
        }

        private void AK_radioButton3_CheckedChanged(object sender, EventArgs e)
        {
            AK_pic1.SizeMode = PictureBoxSizeMode.AutoSize;
        }

        private void AK_radioButton4_CheckedChanged(object sender, EventArgs e)
        {
            AK_pic1.Width = w;
            AK_pic1.Height = h;
            AK_pic1.SizeMode = PictureBoxSizeMode.CenterImage;
        }

        private void AK_radioButton5_CheckedChanged(object sender, EventArgs e)
        {
            AK_pic1.Width = w;
            AK_pic1.Height = h;
            AK_pic1.SizeMode = PictureBoxSizeMode.Zoom;
        }

        private void AK_pic1_MouseDown(object sender, MouseEventArgs e)
        {
            string t = e.Button.ToString();     // Loeb sisse, mis nupp on vajutatud
            if (t != "Left") return;            // Kui ei vajuta vasakut, siis läheb välja
            if (pic != null)
            {
                Point pp = AK_Koordinaadid(e.X, e.Y);
                x1 = pp.X;
                y1 = pp.Y;
            }
            else return;
            AK_groupBox1.Text = x1.ToString() + " " + y1.ToString();
        }

        private void AK_pic1_MouseMove(object sender, MouseEventArgs e)
        {
            string t = e.Button.ToString();     // Loeb sisse, mis nupp on vajutatud
            if (t != "Left") return;            // Kui ei vajuta vasakut, siis läheb välja
            if (pic != null)
            {
                Point pp = AK_Koordinaadid(e.X, e.Y);
                x2 = pp.X;
                y2 = pp.Y;
            }
            else return;
            AK_groupBox1.Text = x2.ToString() + " " + y2.ToString();
            AK_Draw_Rectangle(Color.Red, 3);
        }

        private void AK_Draw_Rectangle(Color c, int k)
        {
            Bitmap bm = new Bitmap(pic);
            Graphics gr = Graphics.FromImage(bm);
            Pen pencil = new Pen(c, k);
            int x0 = Math.Min(x1, x2);
            int y0 = Math.Min(y1, y2);
            int dx = Math.Abs(x1 - x2);
            int dy = Math.Abs(y1 - y2);
            gr.DrawRectangle(pencil, x0, y0, dx, dy);
            AK_pic1.Image = bm;

            if (dx < 1 || dy < 1) return;

            Bitmap area = new Bitmap(dx, dy);
            Graphics g = Graphics.FromImage(area);
            Rectangle in_rect = new Rectangle(x0, y0, dx, dy);
            Rectangle out_rect = new Rectangle(0, 0, dx, dy);
            g.DrawImage(pic, out_rect, in_rect, GraphicsUnit.Pixel);
            AK_pic2.Image = area;
        }

        private Point AK_Koordinaadid(int x, int y)
        {
            Point p = new Point(0, 0);
            if (AK_radioButton1.Checked || AK_radioButton3.Checked)
            {
                p.X = x;
                p.Y = y;
            }
            if (AK_radioButton2.Checked)
            {
                float kx = (float)pic.Width / AK_pic1.Width;
                float ky = (float)pic.Height / AK_pic1.Height;
                p.X = (int)(x * kx);
                p.Y = (int)(y * ky);
            }
            if (AK_radioButton4.Checked)
            {
                int dx = (AK_pic1.Width - pic.Width) / 2;
                int dy = (AK_pic1.Height - pic.Height) / 2;
                p.X = x - dx;
                p.Y = y - dy;
            }
            if (AK_radioButton5.Checked)
            {
                float kx = (float)pic.Width / AK_pic1.Width;
                float ky = (float)pic.Height / AK_pic1.Height;
                int dx = 0;
                int dy = 0;
                if (kx>ky)
                {
                    ky = kx;
                    dy = (int)(AK_pic1.Height - pic.Height/ky) / 2;
                }
                else
                {
                    kx = ky;
                    dx = (int)(AK_pic1.Width - pic.Width/kx) / 2;
                }
                p.X = (int)((x - dx) * kx);
                p.Y = (int)((y - dy) * ky);
            }
            if (p.X < 0) p.X = 0;
            if (p.X > pic.Width) p.X = pic.Width;
            if (p.Y < 0) p.Y = 0;
            if (p.Y > pic.Height) p.Y = pic.Height;
            return p;
        }


    }
}

```
