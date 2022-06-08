# Use IFTTT to send currency exchange rate mail daily


## Requirement
I want to have a daily mail including currency exchange rate information.

1. Mail subject: include the exchange rate in the mail subject.
1. Mail content: 
    1. Has some links to banks' exchange rate page.
    1. Include some exchange rate history chart.

## Solution
Use ifttt.com

1. Create a new Applet on IFTTT
2. Add a new "If This"

<!-- wp:image {"id":432,"width":382,"height":180,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-8.png"><img src="https://dennys.files.wordpress.com/2022/03/image-8.png?w=605" alt="" class="wp-image-432" width="382" height="180"/></a></figure>
<!-- /wp:image -->

3. Choose "finance" service

<!-- wp:image {"id":416,"width":446,"height":274,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image.png"><img src="https://dennys.files.wordpress.com/2022/03/image.png?w=811" alt="" class="wp-image-416" width="446" height="274"/></a></figure>
<!-- /wp:image -->

4. Choose "Today's exchange rate report"

<!-- wp:image {"id":418,"width":568,"height":280,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-1.png"><img src="https://dennys.files.wordpress.com/2022/03/image-1.png?w=1024" alt="" class="wp-image-418" width="568" height="280"/></a></figure>
<!-- /wp:image -->

5. Choose the input/output currency and trigger time.

<!-- wp:image {"id":420,"width":385,"height":373,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-2.png"><img src="https://dennys.files.wordpress.com/2022/03/image-2.png?w=702" alt="" class="wp-image-420" width="385" height="373"/></a></figure>
<!-- /wp:image -->

6. Add a new "Then That"

<!-- wp:image {"id":434,"width":288,"height":148,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-9.png"><img src="https://dennys.files.wordpress.com/2022/03/image-9.png?w=597" alt="" class="wp-image-434" width="288" height="148"/></a></figure>
<!-- /wp:image -->

7. Choose a "gmail" service

<!-- wp:image {"id":422,"width":329,"height":207,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-3.png"><img src="https://dennys.files.wordpress.com/2022/03/image-3.png?w=666" alt="" class="wp-image-422" width="329" height="207"/></a></figure>
<!-- /wp:image -->

8. If you just want to send yourself an email, you can click the right button. If you want to send to several recipients, you can click left button.

<!-- wp:image {"id":424,"width":484,"height":226,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-4.png"><img src="https://dennys.files.wordpress.com/2022/03/image-4.png?w=609" alt="" class="wp-image-424" width="484" height="226"/></a></figure>
<!-- /wp:image -->

9. This is an example to send your self an email.

<!-- wp:image {"id":426,"width":339,"height":489,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2022/03/image-5.png"><img src="https://dennys.files.wordpress.com/2022/03/image-5.png?w=548" alt="" class="wp-image-426" width="339" height="489"/></a></figure>
<!-- /wp:image -->

9.1 This is my email body

<!-- wp:code -->
<pre class="wp-block-code"><code>As of {{CheckTime}}, 1 {{InputCurrency}} equals {{ExchangeRate}} {{OutputCurrency}}.&lt;br&gt;
&lt;br&gt;
via {{InfoUrl}}

&lt;b&gt;美元匯率:&lt;/b&gt;
&lt;ul&gt;&lt;li&gt;
&lt;a href="https://rate.bot.com.tw/xrt?Lang=zh-TW"&gt;即時&lt;/a&gt;(台銀)&amp;nbsp;
&lt;a href="https://rate.bot.com.tw/xrt/quote/day/USD"&gt;當日&lt;/a&gt;(台銀)&amp;nbsp;
&lt;a href="https://www.xe.com/currencycharts/?from=USD&amp;to=TWD&amp;view=1W"&gt;1周&lt;/a&gt;(XE)&amp;nbsp;
&lt;a href="https://www.xe.com/currencycharts/?from=USD&amp;to=TWD&amp;view=1M"&gt;1個月&lt;/a&gt;(XE)&amp;nbsp;
&lt;a href="https://rate.bot.com.tw/xrt/quote/ltm/USD"&gt;3個月&lt;/a&gt;(台銀)&amp;nbsp;
&lt;a href="https://rate.bot.com.tw/xrt/quote/l6m/USD"&gt;6個月&lt;/a&gt;(台銀)
&lt;/li&gt;&lt;li&gt;
&lt;a href="https://rate.bot.com.tw/xrt/quote/l1y/USD"&gt;1年&lt;/a&gt;(台銀)&amp;nbsp;
&lt;a href="https://www.esunbank.com.tw/bank/personal/deposit/rate/forex/exchange-rate-chart?Currency=USD/TWD"&gt;1年&lt;/a&gt;(玉山)&amp;nbsp;
&lt;a href="https://www.xe.com/currencycharts/?from=USD&amp;to=TWD&amp;view=1Y"&gt;1年&lt;/a&gt;(XE)&amp;nbsp;
&lt;a href="https://rate.bot.com.tw/xrt/quote/l3y/USD"&gt;3年&lt;/a&gt;(台銀)&amp;nbsp;
&lt;a href="https://www.xe.com/currencycharts/?from=USD&amp;to=TWD&amp;view=5Y"&gt;5年&lt;/a&gt;(XE)&amp;nbsp;
&lt;a href="https://www.xe.com/currencycharts/?from=USD&amp;to=TWD&amp;view=10Y"&gt;10年&lt;/a&gt;(XE)&amp;nbsp;
&lt;a href="https://fxtop.com/en/historical-exchange-rates.php?A=1&amp;C1=USD&amp;C2=TWD&amp;DD1=&amp;MM1=&amp;YYYY1=&amp;B=1&amp;P=&amp;I=1&amp;DD2=03&amp;MM2=03&amp;YYYY2=2099&amp;btnOK=Go%21"&gt;1983~現在&lt;/a&gt;(FXTOP)&amp;nbsp;
&lt;/li&gt;&lt;li&gt;
&lt;a href="https://historical.findrate.tw/USD/"&gt;1個月+1年+10年&lt;/a&gt;(FindRate)
&lt;/li&gt;&lt;/ul&gt;&lt;br/&gt;

&lt;b&gt;2022/1/1~2022/12/31:&lt;/b&gt;&lt;br/&gt;
&lt;img src="https://fxtop.com/php/imggraph.php?C1=USD&amp;C2=TWD&amp;A=1&amp;DD1=&amp;MM1=01&amp;YYYY1=2022&amp;DD2=&amp;MM2=12&amp;YYYY2=2022&amp;LANG=en&amp;CJ=0&amp;MM1Y=1&amp;LARGE=&amp;TR=OFF"&gt;&lt;br/&gt;
&lt;b&gt;2021/1/1~2022/12/31:&lt;/b&gt;&lt;br/&gt;
&lt;img src="https://fxtop.com/php/imggraph.php?C1=USD&amp;C2=TWD&amp;A=1&amp;DD1=&amp;MM1=01&amp;YYYY1=2021&amp;DD2=&amp;MM2=12&amp;YYYY2=2022&amp;LANG=en&amp;CJ=0&amp;MM1Y=1&amp;LARGE=&amp;TR=OFF"&gt;&lt;br/&gt;
&lt;b&gt;2020/1/1~2022/12/31:&lt;/b&gt;&lt;br/&gt;
&lt;img src="https://fxtop.com/php/imggraph.php?C1=USD&amp;C2=TWD&amp;A=1&amp;DD1=&amp;MM1=01&amp;YYYY1=2020&amp;DD2=&amp;MM2=12&amp;YYYY2=2022&amp;LANG=en&amp;CJ=0&amp;MM1Y=1&amp;LARGE=&amp;TR=OFF"&gt;&lt;br/&gt;
&lt;b&gt;2000/1/1~2022/12/31:&lt;/b&gt;&lt;br/&gt;
&lt;img src="https://fxtop.com/php/imggraph.php?C1=USD&amp;C2=TWD&amp;A=1&amp;DD1=&amp;MM1=01&amp;YYYY1=2000&amp;DD2=&amp;MM2=12&amp;YYYY2=2022&amp;LANG=en&amp;CJ=0&amp;MM1Y=1&amp;LARGE=&amp;TR=OFF"&gt;&lt;br/&gt;
</code></pre>
<!-- /wp:code -->

## Mail result

<!-- wp:image {"id":430,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/03/image-7.png"><img src="https://dennys.files.wordpress.com/2022/03/image-7.png?w=668" alt="" class="wp-image-430"/></a></figure>
<!-- /wp:image -->

## IFTTT applet location

This is my IFTTT applet: https://ifttt.com/applets/SxPJ2fpv-

## About the exchange rate history chart

I only find 2 websites provides exchange rate chart to embed in the mail content.

1. https://fxtop.com/, it provides PNG format chart, I already include it in my IFTTT applet.
1. https://www.currencyconverterrate.com/, it provides SVG format chart (ex: https://www.currencyconverterrate.com/currencycharts/usd/usd-twd-exchange-rates-history-chart-7-day.svg), but I cannot find a solution to show SVG in gmail. If you find a solution, please let me know, thanks.

