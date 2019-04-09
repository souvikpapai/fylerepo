using System;
using System.Net;
using Newtonsoft.Json;

namespace Get_BankDetails_Using_IFSC_Code
{
    class Program
    {
        public class BankDet
        {
            public string bank { get; set; }
            public string ifsc { get; set; }
            public string micr { get; set; }
            public string branch { get; set; }
            public string address { get; set; }
            public string contact { get; set; }
            public string city { get; set; }
            public string district { get; set; }
            public string state { get; set; }
        }
      public void ReadBankDetails(string ifsc_code)
        {
            using (var client = new WebClient())
            {
                client.Headers.Add("DY-X-Authorization: API_KEY");
                var result = client.DownloadString("https://ifsc.datayuge.com/api/v1/" + ifsc_code.Trim());                

                var bankData = JsonConvert.DeserializeObject<BankDet>(result);
                string printres = string.Format("Bank Name : {0}\nIFSC Code : {1}\nMICR : {2}\nBranch : {3}\nAddress : {4}\nContact : {5}\nCity : {6}\nDistrict : {7}\nState : {8}", bankData.bank, bankData.ifsc, bankData.micr, bankData.branch, bankData.address, bankData.contact, bankData.city, bankData.district, bankData.state);
                Console.WriteLine(printres);
            }
        }
        static void Main(string[] args)
        {
            Program obj = new Program();          
            obj.ReadBankDetails("ABHY0065001");
            Console.ReadLine();
        }
    }
}