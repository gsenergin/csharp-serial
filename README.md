using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.IO.Ports;
using System.Threading.Tasks;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        //seriPort isminde bir SerialPort Nesnesi oluşturuluyor
        SerialPort seriPort;
        
        public Form1()
        {
            InitializeComponent();
            seriPort = new SerialPort();
            //serialport nesnesinin hızı 9600 bps olarak ayarlanıyor
            seriPort.BaudRate = 9600;
        }
        //bağlantıyı kur butonunun kodları
        private void button1_Click(object sender, EventArgs e)
        {
            //timer nesnesini başlatıyoruz
            timer1.Start();
            try
            {
                //seriPort nesnesinin ismini text box ile okuyoruz
                seriPort.PortName = textBox1.Text;
                //seriportu açıyoruz
                if (!seriPort.IsOpen)
                {
                    seriPort.Open();
                }
                //bağlantı kuruldu mesajını ekrana veriyoruz
                MessageBox.Show("Bağlantı Kuruldu...");
            }
            catch
            {
                //bağlantı kurulamadı mesajını ekrana veriyoruz
                MessageBox.Show("Bağlantı kurulamadı!");
            }
        }

        private void button2_Click(object sender, EventArgs e)
        {
            //timer nesnesinin çalılmasını durduruyoruz
            timer1.Stop();
            //seriportu kapatıyoruz
            seriPort.Close();
        }

        //Timer Nesnesinin kodlarını yazıyoruz
        //bu kodlar timer nesnesinin her bir tıkında çalışacak
        private void timer1_Tick(object sender, EventArgs e)
        {
            try
            {
                seriPort.Write("a");
                int receiveddata = Convert.ToInt16(seriPort.ReadExisting());
                textBox2.Text = receiveddata.ToString();
                // progressBar nesnesinin doluluk oranı ölçülen potansiyometre değeri ile orantılanıyor
                progressBar1.Value = Convert.ToInt16(textBox2.Text);
                System.Threading.Thread.Sleep(100);
            }
            catch(Exception ex){}
        }
    }
}
