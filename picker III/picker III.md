In the picker III, we have limited function to call (there are 4 predefined functions). However, none of these offer a possibility to call function win. However, none of these offer a possibility to call win(). There is still a gap that can be exploited through write\_variable.


The python code of write\_function will first ask for function to be overwritten and then the new name for that same function. This is how we insert `win()`. I chose `getRandomNumber` as the target since I don't see it as useful here; however, any other table entry is also valid

This is made possible because of**:**

`exec('global ' + var_name + '; ' + var_name + ' = ' + value)`

When we provide the input in order above, we can execute: 

`global getRandomNumber; getRandomNumber = win`

This code declare `getRandomNumber` as global variable and then assign fucntion object win to the name `getRandomNumber`

Remark: I also noticed that in the beginning, the 'read\_variable' will return address of `getRandomNumber` as `0x7169ef692d30` and of `win` as `0x7169ef692dc0`. After the exploitation, the `read variable` return address of both `getRandomNumber` win as `0x7169ef692dc0`

After the modification, by entering 4, we receive a dump of hex values. Converting this dump shall give us the flag:

`picoCTF{7h15\_15\_wh47\_w3\_g37\_w17h\_u53r5\_1n\_ch4rg3\_c20f5222}`

