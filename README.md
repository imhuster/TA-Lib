# TA-Lib

# 简介：
TA-Lib是一个被广泛应用于交易软件、金融市场数据进行技术分析的工具，它提供了C/C++, Java, Perl, Python 以及.NET等语言的支持接口。

然而API函数明显地充斥着C语言的风格，对于Java等面向对象语言开发者不太友好，因此在本人对TA-Lib进行了二次封装，以符合Java语言的规范。其次，对官网的一些函数进行了分类介绍以方便没有相关行业背景的开发者进行使用。

TA-Lib官网 ：http://ta-lib.org/

相关文档：http://ta-lib.org/d_api/d_api.html

## 快速开始

Maven用户：

```XML
<dependency>
    <groupId>org.clojars.edbond</groupId>
    <artifactId>talib</artifactId>
    <version>0.5.0</version>
</dependency>
```
Gradle用户：

```Gradle
compile group: 'org.clojars.edbond', name: 'talib', version: '0.5.0'
```
一般用户：[jar包地址](http://clojars.org/repo/org/clojars/edbond/talib/0.5.0/talib-0.5.0.jar)



数组求和sum:

```java

import com.tictactec.ta.lib.Core;
import com.tictactec.ta.lib.MInteger;
import com.tictactec.ta.lib.RetCode;

/**
 * @Description
 * @author Aven Liu
 * @version 0.01
 */
public class HelloWorld {
	public static void main(String[] args) {
		double[] inReal = { 1, 2, 3, 4, 5, 6, 7 };
		int beginIdx = 0, endIdx = inReal.length, slidingWindow = inReal.length;
		MInteger outBegIdx = new MInteger();
		MInteger outNBElement = new MInteger();
		int retSize = endIdx - beginIdx - slidingWindow + 1;
		double[] outReal = new double[retSize];
		Core core = new Core();
		RetCode retCode = core.sum(beginIdx, endIdx - 1, inReal, slidingWindow, outBegIdx, outNBElement, outReal);
		if (retCode.equals(RetCode.Success)) {
			System.out.println(outReal[0]);
		}
	}
}

```

TA-Lib的API看起来是相当繁琐的，其中Core为TA-Lib的核心类库，其中定义的所有的方法都封装在这个类中，共计501个，方法调用的一般格式如下：

```java
public RetCode sum(int startIdx,
      int endIdx,
      double inReal[],
      int optInTimePeriod,
      MInteger outBegIdx,
      MInteger outNBElement,
      double outReal[])
```

1. startIdx与endIdx为需要进行计算的数组inReal的范围，值得注意的是与Java一般习惯不同的是Ta-Lib包括了endIdx在内。
2. inReal 输入计算数组
3. optInTimePeriod：可以理解为滑动窗口的大小
4. outBegIdx 输出结果outReal的开始下标
5. outNBElement  输出结果outReal的数目
6. outReal 为输出结果数组

MInteger为Ta-Lib自定义的 Integer的一个包装类，以方便修改Integer的值，其定义如下：

```java
public class MInteger{
    public int value;           
}
```

返回值 RetCode 为枚举类，其定义如下：

```java
public enum RetCode{
	Success,
	BadParam,
	OutOfRangeStartIndex,
    OutOfRangeEndIndex,
    AllocErr,
    InternalError
};
```

## TA-Lib 函数介绍

TA-Lib常用的指标及函数可以参考官网： http://ta-lib.org/function.html

Core中的函数共计501个，这些函数分为以下9大类：

#### 数学函数（Math Functions）

```
---------三角函数---------
SIN                  正弦
COS                  余弦
TAN                  正切
ASIN                 反正弦
ACOS                 反余弦
ATAN                 反正切值
SINH                 双曲正弦
COSH                 双曲正弦
TANH                 双曲正切
----指数、对数、取整、根号----
EXP                  指数曲线
LN                   自然对数
LOG10                以10为低的对数
SQRT                 非负实数的平方根
CEIL                 向上取整数
FLOOR                向下取整数
-----向量(一维数组)运算-----
ADD                  向量加法
SUB                  向量减法
MULT                 向量乘法
DIV                  向量除法
SUM                  求和
MAX                  最大值
MIN                  最小值
MAXINDEX             最大值的索引
MININDEX             最小值的索引
MINMAX               最小值和最大值 
MINMAXINDEX          最小值和最大值索引
```

#### 统计函数（Statistic Functions）

```
VAR                  方差（Variance）
STDDEV               标准偏差（Standard Deviation）
BETA                 β系数，贝塔系数（Beta）
CORREL               皮尔逊相关系数（Pearson's Correlation Coefficient）
LINEARREG            线性回归（Linear Regression）
LINEARREG_ANGLE      线性回归的角度（Linear Regression Angle）
LINEARREG_INTERCEPT  线性回归截距 （Linear Regression Intercept）
LINEARREG_SLOPE      线性回归斜率指标（Linear Regression Slope）
TSF                  时间序列预测（Time Series Forecast）
```
#### 重叠研究 （Overlap Studies）

```
BBANDS               Bollinger Bands 
DEMA                 Double Exponential Moving Average 
EMA                  Exponential Moving Average 
HT_TRENDLINE         Hilbert Transform - Instantaneous Trendline 
KAMA                 Kaufman Adaptive Moving Average 
MA                   Moving average 
MAMA                 MESA Adaptive Moving Average  
MAVP                 Moving average with variable period 
MIDPOINT             MidPoint over period 
MIDPRICE             Midpoint Price over period 
SAR                  Parabolic SAR 
SAREXT               Parabolic SAR - Extended 
SMA                  Simple Moving Average 
T3                   Triple Exponential Moving Average (T3) 
TEMA                 Triple Exponential Moving Average 
TRIMA                Triangular Moving Average 
WMA                  Weighted Moving Average 
```

#### 动量指标 （Momentum Indicators）

```
ADX                  Average Directional Movement Index 
ADXR                 Average Directional Movement Index Rating 
APO                  Absolute Price Oscillator 
AROON                Aroon 
AROONOSC             Aroon Oscillator 
BOP                  Balance Of Power 
CCI                  Commodity Channel Index 
CMO                  Chande Momentum Oscillator 
DX                   Directional Movement Index 
MACD                 Moving Average Convergence/Divergence
MACDEXT              MACD with controllable MA type
MACDFIX              Moving Average Convergence/Divergence Fix 12/26
MFI                  Money Flow Index
MINUS_DI             Minus Directional Indicator
MINUS_DM             Minus Directional Movement
MOM                  Momentum
PLUS_DI              Plus Directional Indicator
PLUS_DM              Plus Directional Movement
PPO                  Percentage Price Oscillator
ROC                  Rate of change : ((price/prevPrice)-1)*100
ROCP                 Rate of change Percentage: (price-prevPrice)/prevPrice
ROCR                 Rate of change ratio: (price/prevPrice)
ROCR100              Rate of change ratio 100 scale: (price/prevPrice)*100
RSI                  Relative Strength Index
STOCH                Stochastic
STOCHF               Stochastic Fast
STOCHRSI             Stochastic Relative Strength Index
TRIX                 1-day Rate-Of-Change (ROC) of a Triple Smooth EMA
ULTOSC               Ultimate Oscillator
WILLR                Williams' %R
```

####  成交量指标（Volume Indicators）

```
AD                   Chaikin A/D Line
ADOSC                Chaikin A/D Oscillator
OBV                  On Balance Volume
```

#### 波动性指标（Volatility Indicators）

```
ATR                  Average True Range
NATR                 Normalized Average True Range
TRANGE               True Range
```

#### 价格指标（Price Transform）

```
AVGPRICE             Average Price
MEDPRICE             Median Price
TYPPRICE             Typical Price
WCLPRICE             Weighted Close Price
```


#### 周期指标 （Cycle Indicators）

```
HT_DCPERIOD          Hilbert Transform - Dominant Cycle Period
HT_DCPHASE           Hilbert Transform - Dominant Cycle Phase
HT_PHASOR            Hilbert Transform - Phasor Components
HT_SINE              Hilbert Transform - SineWave
HT_TRENDMODE         Hilbert Transform - Trend vs Cycle Mode
```

#### 形态识别（Pattern Recognition）

```
CDL2CROWS            Two Crows
CDL3BLACKCROWS       Three Black Crows
CDL3INSIDE           Three Inside Up/Down
CDL3LINESTRIKE       Three-Line Strike
CDL3OUTSIDE          Three Outside Up/Down
CDL3STARSINSOUTH     Three Stars In The South
CDL3WHITESOLDIERS    Three Advancing White Soldiers
CDLABANDONEDBABY     Abandoned Baby
CDLADVANCEBLOCK      Advance Block
CDLBELTHOLD          Belt-hold
CDLBREAKAWAY         Breakaway
CDLCLOSINGMARUBOZU   Closing Marubozu
CDLCONCEALBABYSWALL  Concealing Baby Swallow
CDLCOUNTERATTACK     Counterattack
CDLDARKCLOUDCOVER    Dark Cloud Cover
CDLDOJI              Doji
CDLDOJISTAR          Doji Star
CDLDRAGONFLYDOJI     Dragonfly Doji
CDLENGULFING         Engulfing Pattern
CDLEVENINGDOJISTAR   Evening Doji Star
CDLEVENINGSTAR       Evening Star
CDLGAPSIDESIDEWHITE  Up/Down-gap side-by-side white lines
CDLGRAVESTONEDOJI    Gravestone Doji
CDLHAMMER            Hammer
CDLHANGINGMAN        Hanging Man
CDLHARAMI            Harami Pattern
CDLHARAMICROSS       Harami Cross Pattern
CDLHIGHWAVE          High-Wave Candle
CDLHIKKAKE           Hikkake Pattern
CDLHIKKAKEMOD        Modified Hikkake Pattern
CDLHOMINGPIGEON      Homing Pigeon
CDLIDENTICAL3CROWS   Identical Three Crows
CDLINNECK            In-Neck Pattern
CDLINVERTEDHAMMER    Inverted Hammer
CDLKICKING           Kicking
CDLKICKINGBYLENGTH   Kicking - bull/bear determined by the longer marubozu
CDLLADDERBOTTOM      Ladder Bottom
CDLLONGLEGGEDDOJI    Long Legged Doji
CDLLONGLINE          Long Line Candle
CDLMARUBOZU          Marubozu
CDLMATCHINGLOW       Matching Low
CDLMATHOLD           Mat Hold
CDLMORNINGDOJISTAR   Morning Doji Star
CDLMORNINGSTAR       Morning Star
CDLONNECK            On-Neck Pattern
CDLPIERCING          Piercing Pattern
CDLRICKSHAWMAN       Rickshaw Man
CDLRISEFALL3METHODS  Rising/Falling Three Methods
CDLSEPARATINGLINES   Separating Lines
CDLSHOOTINGSTAR      Shooting Star
CDLSHORTLINE         Short Line Candle
CDLSPINNINGTOP       Spinning Top
CDLSTALLEDPATTERN    Stalled Pattern
CDLSTICKSANDWICH     Stick Sandwich
CDLTAKURI            Takuri (Dragonfly Doji with very long lower shadow)
CDLTASUKIGAP         Tasuki Gap
CDLTHRUSTING         Thrusting Pattern
CDLTRISTAR           Tristar Pattern
CDLUNIQUE3RIVER      Unique 3 River
CDLUPSIDEGAP2CROWS   Upside Gap Two Crows
CDLXSIDEGAP3METHODS  Upside/Downside Gap Three Methods
```


## 参考
[TA-Lib](http://ta-lib.org/)

[Cryptotrader](https://cryptotrader.org/talib)

[talib-document](https://github.com/HuaRongSAO/talib-document)