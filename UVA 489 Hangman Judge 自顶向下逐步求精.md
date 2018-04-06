# UVA 489 Hangman Judge 自顶向下逐步求精

标签（空格分隔）： 算法 算法竞赛

---
原题如下，但这种做法是我逐步改进的结果，是逐步找到漏洞，逐渐修复的结果。我觉得这个比刘汝佳书上的方法要蠢得多。

        In “Hangman Judge,” you are to write a program that judges a series of Hangman games. For each game, the answer to the puzzle is given as well as the guesses. Rules are the same as the classic game of hangman, and are given as follows:
        1. The contestant tries to solve to puzzle by guessing one letter at a time.
        2. Every time a guess is correct, all the characters in the word that match the guess will be “turned over.” For example, if your guess is ‘o’ and the word is “book”, then both ‘o’s in the solution will be counted as “solved”.
        3. Every time a wrong guess is made, a stroke will be added to the drawing of a hangman, which needs 7 strokes to complete. Each unique wrong guess only counts against the contestant once.

    ——————
    |   |
    |   O
    |  /|\
    |   |
    |  / \
    __
    | |______
    |_________

        4. If the drawing of the hangman is completed before the contestant has successfully guessed all the
        characters of the word, the contestant loses.
        5. If the contestant has guessed all the characters of the word before the drawing is complete, the contestant wins the game.
        6. If the contestant does not guess enough letters to either win or lose, the contestant chickens out.Your task as the “Hangman Judge” is to determine, for each game, whether the contestant wins,loses, or fails to finish a game.
        
        Input
        
        Your program will be given a series of inputs regarding the status of a game. All input will be in lower case. The first line of each section will contain a number to indicate which round of the game is being played; the next line will be the solution to the puzzle; the last line is a sequence of the guesses made by the contestant. A round number of ‘-1’ would indicate the end of all games (and input).
        
        Output
        
        The output of your program is to indicate which round of the game the contestant is currently playing as well as the result of the game. There are three possible results:
        
        You win.
        You lose.
        You chickened out.
        
        Sample Input
        
        1
        cheese
        chese
        2
        cheese
        abcdefg
        3
        cheese
        abcdefgij
        -1
        
        Sample Output
        
        Round 1
        You win.
        Round 2
        You chickened out.
        Round 3
        You lose.
题干很长，大意就是输入一个数字代表回合几，然后输入第一个字符串表示目标字符串a，再输入我们猜测的字符串b。从第一位开始检索，如果b中含有a中的字符，那a中所有这个字符就会显现出来；如果没有，表示猜错了，就会在图上画一笔，画到第七笔时，你就输了；如果检索到了b的最后也没有猜对a，也没有错七次及以上，就显示“You chickened out.”；如果错不超过七次就猜出了a中的所有字母，就赢了。注意，猜已经猜过的字符也算错。

下面是程序。我用的布尔数组，用true和false表示某个字符是否已猜过一次(flag[N])和某个字符是否在a中(spot[N])。

```C
#include <stdio.h>
#include <iostream>
#include <memory.h>
#define N 27

int sum(bool a[N]) 		//计算布尔数组中的真值个数 
{
	int sum=0;
	for (int i=0;i<N;i++)
		if (a[i])
			sum++;
	return sum;
}

int blen(char a[N])		//计算字符数组中的字母种类 
{
	bool sp[N]={false};
	int sum=0;
	for (int i=0;a[i]!='\0';i++)
		sp[a[i]-'a']=true;
	for (int i=0;i<N;i++)
		if (sp[i])
			sum++;
	return sum;
}

int main()
{
	int i,n;
	bool spot[27],flag[27];
	char a[N],b[N];
	while (scanf("%d",&n),n!=-1)
	{
		int key=0;
		printf("Round %d\n",n);
		memset(spot,false,27);
		memset(flag,false,27);
		getchar();
		gets(a);
		gets(b);
		for (i=0;b[i]!='\0';i++)
		{
			if (key==7)				
				break;
			int p=0;
			if (flag[b[i]-'a'])				//判断是否已猜过这个字符
			{
				key++;
				continue;
			}
			else
				flag[b[i]-'a']=true;
			for (int j=0;a[j]!='\0';j++)
			{
				if (spot[a[j]-'a'])
					continue;
				if (b[i]==a[j])
				{
					spot[a[j]-'a']=true;
					p=1;
				}
			}
			if (!p)
				key++;
			if (blen(a)==sum(spot))
			{
				printf("You win.\n");
				goto end;
			}
		}
		if (b[i]||key==7)
		{
			printf("You lose.\n");
			continue;
		}
		if (b[i]=='\0'&&(sum(spot)<blen(a)))
			printf("You chickened out.\n");
		end:;
	}
	return 0;
}
```


