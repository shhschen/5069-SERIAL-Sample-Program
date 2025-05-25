Introduction:
The Allen-Bradley module 5069-SERIAL, released in 2018, offers enhanced communication capabilities for various protocols, including DH-485 and DF1, through a firmware update V2.012 in 2020. While the user manual lacks practical examples for sensor communication, this comprehensive application note aims to fill that gap. Specifically, we will focus on integrating the module with the Panasonic HL-G112 Laser Displacement Sensor. By following the outlined setup process, you can avoid pitfalls and accelerate your prototype development.

Allen-Bradley 5069-SERIAL模塊於2018年推出，通過2020年的固件更新V2.012，為各種協議（包括DH-485和DF1）提供了增強的通訊功能。然而，該用戶手冊缺乏傳感器通訊的實用示例，本全面的應用指南旨在填補這一空白。具體來說，我們將重點介紹如何將此模塊與Panasonic HL-G112雷射位移傳感器集成。通過按照所述的設置過程，您可以避免陷阱，加速原型開發。

Section 1: Understanding Sensor Specifications:
In this Application Note, I pick Panasonic HL-G112 Laser Displacement Sensor for development. I'll walk you through the entire setup process so that you can stay away from traps and have your prototype hit the ground running earlier!

在本應用指南中，我選擇了Panasonic HL-G112雷射位移傳感器進行開發。我將引導您完成整個設置過程，以使您遠離陷阱，讓您的原型更早落地運行！

Let's start with the sensor specification sheet. (Download Link) I use RS-485 in this Application Note because I'll have more than 10 receivers in my system. You can choose RS-422 if a full-duplex serves you better. Pay attention to the Transmission code, Baud rate, Data Length, Stop bit length, Parity check, BCC, and Termination code. (Basically everything in the table below.)

讓我們從傳感器規格表開始（Download Link）。在這個應用指南中，我使用RS-485，因為我在系統中有超過10個接收器。如果full-duplex適合您，您可以選擇RS-422。注意傳輸碼、波特率、數據長度、停止位長度、奇偶校驗、BCC和終止碼。（基本上表中的所有內容。）

The figure below explains how to wire these sensor modules to your PLC controller in the RS-485 protocol. If you choose RS-422, you will have different wiring. Remember, RTFDS. (A quote from James Bryant. It means "Read the friendly data sheet.")

下圖解釋了如何將這些傳感器模塊連接到RS-485協議的PLC控制器。如果您選擇RS-422，則將有不同的連接方式。記住，RTFDS。（James Bryant的名言，意思是“閱讀友好的數據表”。）

Section 2: Examining Request and Response Strings
After knowing the communication specification and wiring, let's take a look at the string they use for transmitting requests and receiving data. (HL-G1 Series User Manual) 

在了解通訊規格和連接方式之後，讓我們來看看他們用於傳輸請求和接收數據的字符串。（HL-G1 Series User Manual）

Oh, by the way, you have to press three buttons on the sensor and configure some parameters based on your application. I'm not going to put too much attention here. If you have questions regarding this specific sensor, you can comment below or contact the manufacturer.

哦，對了，您需要按壓傳感器上的三個按鈕並根據您的應用程序配置一些參數。這裡不會花太多精力介紹。如果對這款特定傳感器有問題，可以在下面評論或聯繫製造商。

In Fig. 5, you can see a specific Read Command for this sensor. I underlined Start delimiter and Termination delimiter myself since they're key to later implementation but not explained in either manual.

在圖5中，您可以看到該傳感器的特定讀取命令。我自己在開始分隔符和終止分隔符加上紅色底線，因為它們對於後續實施非常重要，但在任何手冊中都沒有解釋。

Section 3: Setting up the 5069-SERIAL Module
In Fig. 6, you see a normal response string from this sensor. Start delimiter and Termination delimiter are highlighted as well. Now, let's see how to set up the 5069-SERIAL module. (I skipped how to create a project and configure a controller because that's a common tutorial topic.)

在圖6中，您可以看到來自該傳感器的正常響應字符串。同樣，開始分隔符和終止分隔符也被突出顯示。現在，讓我們看看如何設置5069-SERIAL模塊。（我跳過了如何創建項目和配置控制器，因為這是常見的教程主題。）

In the General tab, check your firmware revision. V2.012 is the latest version when I write this Application Note. Besides, configure the communication method based on your need. I pick Generic ASCII because that's the only type supported by this sensor.

在常規選項卡中，檢查您的韌體版本。在我撰寫本應用指南時，V2.012是最新版本。此外，根據您的需求配置通訊方法。我選擇通用ASCII，因為這是該傳感器唯一支持的類型。

Remember Fig. 3 Communication Specification above? Filled in those parameters to the blanks in Fig. 8 and you're good to go.

還記得上面的圖3通訊規格嗎？將這些參數填入圖8中的空格，然後就可以開始了。 

Section 4: Configuring Start and Termination Delimiters
Here comes the critical/tricky part! In the drop-down lists in Fig. 9 and Fig. 10, you have Ignore Start Delimiter, Exclude, and Include. Their definitions are in Fig. 11. (Source: Compact 5000 I/O Serial Module)

這就是關鍵和棘手的部分！在圖9和圖10的下拉列表中，您有忽略開始分隔符、排除和包含等選項。它們的定義在圖11中。（來源：Compact 5000 I/O Serial Module）

Look at the Receive Function first. If you keep the default setting "Ignore Start Delimiter" for the Start Mode, you only know it's not based on Start Delimiter. (Termination Mode is a bit clearer on this though.) Then what is it based on? I don't know. Maybe count backward the bytes you specified once it sees a Termination Delimiter? 

首先看接收功能。如果您在開始模式中保持默認設置“忽略開始分隔符”，那麼您只知道它不是基於開始分隔符的。（終止模式在這方面稍微清楚一些。）那麼它是基於什麼的？我不知道。也許一旦它看到終止分隔符，可能會向後計算指定的字節數？

To stay away from ambiguity, I choose to Include a Start Delimiter and a Termination Delimiter so that I can see them in my input tag.

為避免模棱兩可，我選擇包含開始分隔符和終止分隔符，以便在我的輸入標籤中可以看到它們。

For the transmit function, the controller by default transmits and receives packets based on bytes. But here I choose the Include Mode for the same reason. Keep it clear.

對於傳輸功能，控制器默認按字節傳輸和接收數據包。但在這裡，我出於同樣的原因選擇了包含模式。保持清晰。

After figuring out what Start Mode and Termination Mode we need, let's deal with the delimiters. For the Start delimiter, we know it's %. And the Termination delimiter is CR (Carriage Return). We then look up the ASCII table (Source: https://www.rapidtables.com/code/text/ascii-table.html) and find out their corresponding HEX representations.

在確定所需的開始模式和終止模式之後，讓我們處理分隔符。開始分隔符我們知道是%。而終止分隔符是CR（回車）。然後查找ASCII表（來源：https://www.rapidtables.com/code/text/ascii-table.html），找出它們對應的十六進制表示。

Now you know why I place $25 and $0D in the blanks in Fig. 9 and Fig. 10, right? (If you're not using the second Termination delimiter, please place $FF in the box.) Then let's move forward to the PLC program itself.

現在您知道為什麼我在圖9和圖10的空格中放置了$25和$0D，對吧？（如果您不使用第二個終止分隔符，請在框中放置$FF。）然後，讓我們繼續進行PLC程序本身。

Section 5: Implementing the PLC Program
I use S:FS to initialize the system. You can use something else if that fits your applications. Then take a closer look at the string on the lower right corner. (Review Fig. 5 if you don't remember the data request string.) Once you get this part right, you're 99% done. 

我使用S:FS來初始化系統。如果這符合您的應用程序，您也可以使用其他內容。然後仔細查看右下角的字符串。（如果您不記得數據請求字符串，請查看圖5。）一旦這部分正確，您就已經完成了99％的工作。

Section 6: Calculating the Block Check Character (BCC)
"But how do you get the BCC (Block Check Character) result?" you may ask. No worries, I've got you covered. Simply click this link (https://bcc.beyerleinf.de/) and type in your string and voila! You have your unique BCC calculated.

「但是，您如何獲得BCC（塊校驗字符）的結果？」您可能會問。別擔心，我幫您解決。只需點擊此鏈接（https://bcc.beyerleinf.de/），輸入字符串，然後驚喜地獲得獨特的BCC計算結果。

Section 7: Finalizing the Configuration and Handling Alarms
In Fig. 16, I think everything is self-explained. (If not, feel free to drop down your question in the comment below.) In a normal operating condition, the controller will get updates from the sensor every second. Then it converts a string into a double integer; then a double integer to a real number.

在圖16中，我認為一切都是足以自行解釋的。（如果不是，請放心在下面的評論中提問。）在正常運行條件下，控制器每秒從傳感器獲得更新。然後將字符串轉換為雙整數，再轉換為實數。

If the communication is sabotaged somehow or the sensor is dead, the value displayed on HMI will be set to zero. In the meantime, you can trigger an alarm and have people take care of it.

如果通信某種原因被破壞或傳感器失靈，HMI顯示的值將被設置為零。同時，您可以觸發警報並讓人員加以處理。

Conclusion:
Till this point, you should have your prototype up and running. I hope this Application Note can save you time when developing your own program. If you want to download my sample code, please visit my GitHub page.

到此為止，您應該已經完成了原型的設置和運行。我希望本應用指南可以幫助您在開發自己的程序時節省時間。如果您想下載我的示例代碼，請訪問我的GitHub頁面。
