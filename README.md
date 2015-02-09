
## 溫度感測器搭配四位數顯示器

* 實作簡易環境溫度感測器並將即時監控的溫度顯示在LED顯示器上．

## 電子零件介紹

* Arch Pro 開發板 ( 同樣採用 mbed  LPC1768 )
* Base Shield 
* Grove Temperature 
* Grove 4-Digit-Display 
* Grove - Universal 4 Pin 20cm Unbuckled Cable

![圖1：溫度感測器實作採用零件 ](/Users/Joker/Downloads/Arch pro shield and sensor.jpg)

## 溫度感測器實作步驟

* 將 Base Shield 接在 Arch Pro 上 
* 溫度感測器接 Universal 4 pin Cable 線，並接在 Base Shield 的 A0 
* 將四位數LED顯示器接 Universal 4 pin Cable 線，並接在 Base Shield 的 UART
* 記得將 Base Shield 切換至 5V VCC 

![圖2：完成圖](/Users/Joker/Downloads/Temp_Digit Display_Part.jpg)

## 撰寫程式碼

```
#include "mbed.h"
#include "DigitDisplay.h"

DigitalOut myled(LED2);

DigitDisplay display(P4_29, P4_28);
AnalogIn ain(P0_23);

int a;
float temperature;
int B=3975;                                                         //B value of the thermistor
float resistance;

int main()
{
    while(1) {
// multiply ain by 675 if the Grove shield is set to 5V or 1023 if set to 3.3V
        a=ain*675;
        resistance=(float)(1023-a)*10000/a;                         //get the resistance of the sensor;
        temperature=1/(log(resistance/10000)/B+1/298.15)-273.15;    //convert to temperature via datasheet ;
        myled = 1;
        wait(0.8);
        myled = 0;
        wait(0.8);
        display.write(temperature);
    }
}

```
## 即時溫度監控顯示畫面 

* 由程式碼定義 Arch Pro 的第二顆 Led 紅燈閃爍來簡易判斷是否有正常執行運作。
![圖3：溫度監控即時顯示LED實作](/Users/Joker/Downloads/temperature_detector_prototype.jpg)

## 參考資源
* http://developer.mbed.org/users/djbottrill/code/Grove_Thermometer/

