# context_retrieval
## Import relevant libraries


```python
import requests
import pandas as pd
import numpy as np
from bs4 import BeautifulSoup
```

## Setting up scraping for the given website using requests and BeautifulSoup


```python
random_lib = "https://docs.python.org/3/library/random.html"
res_r = requests.get(random_lib)
soup = BeautifulSoup(res_r.content, 'html.parser')
```


```python
functions = soup.find_all('dt',{'class':'sig sig-object py'})
```


```python
import re
function_names=[a[4:] for a in re.findall('id="random.\w+',str(BeautifulSoup(res_r.content)))][1:-1]
function_names
```




    ['random.seed',
     'random.getstate',
     'random.setstate',
     'random.randbytes',
     'random.randrange',
     'random.randint',
     'random.getrandbits',
     'random.choice',
     'random.choices',
     'random.shuffle',
     'random.sample',
     'random.random',
     'random.uniform',
     'random.triangular',
     'random.betavariate',
     'random.expovariate',
     'random.gammavariate',
     'random.gauss',
     'random.lognormvariate',
     'random.normalvariate',
     'random.vonmisesvariate',
     'random.paretovariate',
     'random.weibullvariate',
     'random.Random',
     'random.SystemRandom']




```python
function_descriptions = [" ".join(a.text.split("\n")) for a in soup.find_all('dd')]
```


```python
functions_df = pd.DataFrame({'function_names':function_names, 'function_description':function_descriptions})
functions_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>function_names</th>
      <th>function_description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>random.seed</td>
      <td>Initialize the random number generator. If a i...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>random.getstate</td>
      <td>Return an object capturing the current interna...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>random.setstate</td>
      <td>state should have been obtained from a previou...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>random.randbytes</td>
      <td>Generate n random bytes. This method should no...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>random.randrange</td>
      <td>Return a randomly selected element from range(...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>random.randint</td>
      <td>Return a random integer N such that a &lt;= N &lt;= ...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>random.getrandbits</td>
      <td>Returns a non-negative Python integer with k r...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>random.choice</td>
      <td>Return a random element from the non-empty seq...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>random.choices</td>
      <td>Return a k sized list of elements chosen from ...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>random.shuffle</td>
      <td>Shuffle the sequence x in place. To shuffle an...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>random.sample</td>
      <td>Return a k length list of unique elements chos...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>random.random</td>
      <td>Return the next random floating point number i...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>random.uniform</td>
      <td>Return a random floating point number N such t...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>random.triangular</td>
      <td>Return a random floating point number N such t...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>random.betavariate</td>
      <td>Beta distribution.  Conditions on the paramete...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>random.expovariate</td>
      <td>Exponential distribution.  lambd is 1.0 divide...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>random.gammavariate</td>
      <td>Gamma distribution.  (Not the gamma function!)...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>random.gauss</td>
      <td>Normal distribution, also called the Gaussian ...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>random.lognormvariate</td>
      <td>Log normal distribution.  If you take the natu...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>random.normalvariate</td>
      <td>Normal distribution.  mu is the mean, and sigm...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>random.vonmisesvariate</td>
      <td>mu is the mean angle, expressed in radians bet...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>random.paretovariate</td>
      <td>Pareto distribution.  alpha is the shape param...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>random.weibullvariate</td>
      <td>Weibull distribution.  alpha is the scale para...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>random.Random</td>
      <td>Class that implements the default pseudo-rando...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>random.SystemRandom</td>
      <td>Class that uses the os.urandom() function for ...</td>
    </tr>
  </tbody>
</table>
</div>




```python
functions_df.to_csv('python_random_functions.csv', index = False)
```


```python
bookkeeping = soup.find_all('section', {'id':'bookkeeping-functions'})
```


```python
bookkeeping
```




    [<section id="bookkeeping-functions">
     <h2>Bookkeeping functions<a class="headerlink" href="#bookkeeping-functions" title="Permalink to this headline">¶</a></h2>
     <dl class="py function">
     <dt class="sig sig-object py" id="random.seed">
     <span class="sig-prename descclassname"><span class="pre">random.</span></span><span class="sig-name descname"><span class="pre">seed</span></span><span class="sig-paren">(</span><em class="sig-param"><span class="n"><span class="pre">a</span></span><span class="o"><span class="pre">=</span></span><span class="default_value"><span class="pre">None</span></span></em>, <em class="sig-param"><span class="n"><span class="pre">version</span></span><span class="o"><span class="pre">=</span></span><span class="default_value"><span class="pre">2</span></span></em><span class="sig-paren">)</span><a class="headerlink" href="#random.seed" title="Permalink to this definition">¶</a></dt>
     <dd><p>Initialize the random number generator.</p>
     <p>If <em>a</em> is omitted or <code class="docutils literal notranslate"><span class="pre">None</span></code>, the current system time is used.  If
     randomness sources are provided by the operating system, they are used
     instead of the system time (see the <a class="reference internal" href="os.html#os.urandom" title="os.urandom"><code class="xref py py-func docutils literal notranslate"><span class="pre">os.urandom()</span></code></a> function for details
     on availability).</p>
     <p>If <em>a</em> is an int, it is used directly.</p>
     <p>With version 2 (the default), a <a class="reference internal" href="stdtypes.html#str" title="str"><code class="xref py py-class docutils literal notranslate"><span class="pre">str</span></code></a>, <a class="reference internal" href="stdtypes.html#bytes" title="bytes"><code class="xref py py-class docutils literal notranslate"><span class="pre">bytes</span></code></a>, or <a class="reference internal" href="stdtypes.html#bytearray" title="bytearray"><code class="xref py py-class docutils literal notranslate"><span class="pre">bytearray</span></code></a>
     object gets converted to an <a class="reference internal" href="functions.html#int" title="int"><code class="xref py py-class docutils literal notranslate"><span class="pre">int</span></code></a> and all of its bits are used.</p>
     <p>With version 1 (provided for reproducing random sequences from older versions
     of Python), the algorithm for <a class="reference internal" href="stdtypes.html#str" title="str"><code class="xref py py-class docutils literal notranslate"><span class="pre">str</span></code></a> and <a class="reference internal" href="stdtypes.html#bytes" title="bytes"><code class="xref py py-class docutils literal notranslate"><span class="pre">bytes</span></code></a> generates a
     narrower range of seeds.</p>
     <div class="versionchanged">
     <p><span class="versionmodified changed">Changed in version 3.2: </span>Moved to the version 2 scheme which uses all of the bits in a string seed.</p>
     </div>
     <div class="versionchanged">
     <p><span class="versionmodified changed">Changed in version 3.11: </span>The <em>seed</em> must be one of the following types:
     <em>NoneType</em>, <a class="reference internal" href="functions.html#int" title="int"><code class="xref py py-class docutils literal notranslate"><span class="pre">int</span></code></a>, <a class="reference internal" href="functions.html#float" title="float"><code class="xref py py-class docutils literal notranslate"><span class="pre">float</span></code></a>, <a class="reference internal" href="stdtypes.html#str" title="str"><code class="xref py py-class docutils literal notranslate"><span class="pre">str</span></code></a>,
     <a class="reference internal" href="stdtypes.html#bytes" title="bytes"><code class="xref py py-class docutils literal notranslate"><span class="pre">bytes</span></code></a>, or <a class="reference internal" href="stdtypes.html#bytearray" title="bytearray"><code class="xref py py-class docutils literal notranslate"><span class="pre">bytearray</span></code></a>.</p>
     </div>
     </dd></dl>
     <dl class="py function">
     <dt class="sig sig-object py" id="random.getstate">
     <span class="sig-prename descclassname"><span class="pre">random.</span></span><span class="sig-name descname"><span class="pre">getstate</span></span><span class="sig-paren">(</span><span class="sig-paren">)</span><a class="headerlink" href="#random.getstate" title="Permalink to this definition">¶</a></dt>
     <dd><p>Return an object capturing the current internal state of the generator.  This
     object can be passed to <a class="reference internal" href="#random.setstate" title="random.setstate"><code class="xref py py-func docutils literal notranslate"><span class="pre">setstate()</span></code></a> to restore the state.</p>
     </dd></dl>
     <dl class="py function">
     <dt class="sig sig-object py" id="random.setstate">
     <span class="sig-prename descclassname"><span class="pre">random.</span></span><span class="sig-name descname"><span class="pre">setstate</span></span><span class="sig-paren">(</span><em class="sig-param"><span class="n"><span class="pre">state</span></span></em><span class="sig-paren">)</span><a class="headerlink" href="#random.setstate" title="Permalink to this definition">¶</a></dt>
     <dd><p><em>state</em> should have been obtained from a previous call to <a class="reference internal" href="#random.getstate" title="random.getstate"><code class="xref py py-func docutils literal notranslate"><span class="pre">getstate()</span></code></a>, and
     <a class="reference internal" href="#random.setstate" title="random.setstate"><code class="xref py py-func docutils literal notranslate"><span class="pre">setstate()</span></code></a> restores the internal state of the generator to what it was at
     the time <a class="reference internal" href="#random.getstate" title="random.getstate"><code class="xref py py-func docutils literal notranslate"><span class="pre">getstate()</span></code></a> was called.</p>
     </dd></dl>
     </section>]




```python

```
