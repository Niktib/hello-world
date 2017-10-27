using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {            
            //string sourceFile = @"C:\Users\Public\public\test.txt";
            //string destinationFile = @"C:\Users\Public\private\test.txt";
            //// To move a file or folder to a new location:
            //File.Move(sourceFile, destinationFile);

            //// To move an entire directory. To programmatically modify or combine
            //// path strings, use the System.IO.Path class.
            //Directory.Move(@"C:\Users\Public\public\test\", @"C:\Users\Public\private");

            MessageBox.Show(MonthAndDate());
        }
        private void MovingFiles() //This just moves the files inside of a folder
        {
            string[] transactionTypes = {"New_Business", "Endorsement", "Cancellation", "Renewal" };
            string[] lines = File.ReadAllLines(Path.GetDirectoryName(Application.ExecutablePath) + @"\FileDirectory.txt");
            string[] ToFromArray;
            foreach (string line in lines)
            {
                ToFromArray = line.Split(("|").ToCharArray());
                DirectoryInfo dirInfo = new DirectoryInfo(ToFromArray[1]);
                if (dirInfo.Exists == false)
                    Directory.CreateDirectory(ToFromArray[1]);

                List<String> MyFiles = Directory
                    .GetFiles(ToFromArray[0], "*.*", SearchOption.AllDirectories).ToList();

                foreach (string file in MyFiles)
                {
                    FileInfo mFile = new FileInfo(file);
                    // to remove name collisions
                    if (new FileInfo(dirInfo + "\\" + mFile.Name).Exists == false)
                    {
                        mFile.CopyTo(dirInfo + "\\" + mFile.Name);
                    }
                }
            }
        }
        private void FromIDrivetoPDrive()
        {
            string[] lines = File.ReadAllLines(Path.GetDirectoryName(Application.ExecutablePath) + @"\FileDirectory.txt");
            string[] ToFromArray;
            foreach (string line in lines)
            {
                //I: Drive to P: drive
                ToFromArray = line.Split(("|").ToCharArray()); //Array spot 0 is the WHERE and the 1 is the TO
                //For this to work the destination folder needs to not currently exist.
                Directory.Move(ToFromArray[0] + "\\", ToFromArray[1]);
            }
        } //All done, Deletes first file
        private string MonthAndDate()
        {
            string finalValue;
            string dateDay = dayFileFinder(DateTime.Today.Day.ToString());
            string dateMonth = DateTime.Today.Month.ToString() + " " + DateTime.Now.ToString("MMMM");
            finalValue = "\\" + dateMonth + "\\" + dateDay ;
            return finalValue;
        }
        private string dayFileFinder(string dateDay)
        {
            string FinalValue = dateDay + "th";
            string lastCharacter = dateDay.Substring(dateDay.Length - 1, 1);
            if (lastCharacter == "1" && dateDay != "11")  { FinalValue = dateDay + "st"; }
            if (lastCharacter == "2" && dateDay != "12") { FinalValue = dateDay + "nd"; }
            if (lastCharacter == "3" && dateDay != "13") { FinalValue = dateDay + "rd"; }
            return FinalValue;
        }
    }
}
