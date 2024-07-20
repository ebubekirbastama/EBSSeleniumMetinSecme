Makaleye [buradan](https://www.csharpegitimi.com.tr/2024/06/csharp-selenium-elment-text-secme.html) ulaşabilirsiniz.

# EBSSeleniumMetinSecme

**Proje Adı:** EBSSeleniumMetinSecme

**Açıklama:** Bu proje, Selenium WebDriver kullanarak belirli bir web sayfasındaki metni seçip kopyalamayı sağlayan bir Windows Forms uygulamasıdır. Kullanıcılar, uygulama aracılığıyla bir web sayfasındaki belirli bir div elementindeki metni kopyalayıp, uygulama içinde görüntüleyebilirler.

## Özellikler
- **Web Sayfasına Erişim:** Kullanıcı, uygulama aracılığıyla belirli bir URL'ye (örneğin, "https://x.com/ebubekirstt") yönlendirilir.
- **Metin Seçme:** Belirtilen bir div elementindeki metin, JavaScript kullanılarak seçilir.
- **Metin Kopyalama:** Seçilen metin, Selenium WebDriver ve Actions sınıfı kullanılarak panoya kopyalanır.
- **Metni Görüntüleme:** Panodan alınan metin, uygulama içindeki bir RichTextBox kontrolünde görüntülenir.

## Teknolojiler
- **Programlama Dili:** C#
- **Web Sürüş Aracı:** Selenium WebDriver
- **Tarayıcı:** Chrome (ChromeDriver)
- **Arayüz:** Windows Forms

## Gereksinimler
- **Selenium WebDriver:** En son sürümü yükleyin.
- **ChromeDriver:** Bilgisayarınıza indirin ve ChromeDriver yolunu kodda belirtin.
- **.NET Framework:** Uygun bir sürümü yüklü olmalıdır.
- **Visual Studio:** Projeyi derlemek ve çalıştırmak için.

## Kurulum ve Kullanım
1. Projeyi bilgisayarınıza klonlayın veya indirin:
   ```sh
   git clone https://github.com/ebubekirbastama/EBSSeleniumMetinSecme.git
   ```
2. ChromeDriver'ı [buradan](https://www.nuget.org/packages/Selenium.WebDriver) indirin ve bilgisayarınıza kurun.
3. ChromeDriver'ın yolunu belirtin:
   ```csharp
   IWebDriver driver = new ChromeDriver("chromeDriverPath");
   ```
4. Visual Studio gibi bir IDE'de projeyi açın.
5. Gerekli NuGet paketlerini yükleyin:
   - Selenium.WebDriver
   - Selenium.WebDriver.ChromeDriver
   ```sh
   Install-Package Selenium.WebDriver
   Install-Package Selenium.WebDriver.ChromeDriver
   ```
6. Uygulamayı çalıştırın ve belirtilen URL'ye yönlendirilen web sayfasını açın.

## Kod
```csharp
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Interactions;
using System;
using System.Threading;
using System.Windows.Forms;
using Keys = OpenQA.Selenium.Keys;

namespace EBSSeleniumMetinSecme
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            // ChromeDriver'ın yolunu belirtin
            IWebDriver driver = new ChromeDriver();

            // İlgili web sayfasını açın
            driver.Navigate().GoToUrl("https://x.com/_shadowintel_");
            Thread.Sleep(5000);

            // Belirtilen div elementini bulun
            IWebElement divElement = driver.FindElement(By.CssSelector("div[data-testid='UserDescription']"));

            secimyap(divElement, driver);

            // Tarayıcıyı kapatın
            driver.Quit();
        }

        private void secimyap(IWebElement divElement, IWebDriver driver)
        {
            // JavaScript ile metni seçmek ve kopyalamak
            var jsExecutor = (IJavaScriptExecutor)driver;

            // Elementin metnini seç
            jsExecutor.ExecuteScript(@"
                var element = arguments[0];
                var range = document.createRange();
                range.selectNodeContents(element);
                var selection = window.getSelection();
                selection.removeAllRanges();
                selection.addRange(range);
            ", divElement);

            // Kopyalama işlemi için Control + C tuşlarına basın
            Actions actions = new Actions(driver);
            actions.KeyDown(Keys.Control)
                   .SendKeys("c")
                   .KeyUp(Keys.Control)
                   .Build()
                   .Perform();

            // Panodan metni alın
            string copiedText = Clipboard.GetText();

            richTextBox1.AppendText(copiedText);
        }
    }
}
