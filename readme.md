
##Background Information

In a Stroop task, participants are presented with a list of words, with each word displayed in a color of ink. The participant’s task is to say out loud the color of the ink in which the word is printed. The task has two conditions: a congruent words condition, and an incongruent words condition. In the congruent words condition, the words being displayed are color words whose names match the colors in which they are printed: for example RED, BLUE. In the incongruent words condition, the words displayed are color words whose names do not match the colors in which they are printed: for example PURPLE, ORANGE. In each case, we measure the time it takes to name the ink colors in equally-sized lists. Each participant will go through and record a time from each condition.
Questions For Investigation

###References:
Statistical References:
>    https://en.wikipedia.org/wiki/Student%27s_t-test
>    https://en.wikipedia.org/wiki/Welch%27s_t_test
>    https://en.wikipedia.org/wiki/Stroop_effect#Theories

Programing References:
>    http://stackoverflow.com/questions/19410042/how-to-make-ipython-notebook-inline-matplotlib-graphics
    http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.describe.html
    http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.hist.html
    http://matplotlib.org/api/pyplot_api.html
    http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.probplot.html
    http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.ttest_ind.html


###1. What is our independent variable? What is our dependent variable?

The independent variable is whether or not the words are congruent.
The dependent variable is the time it takes to name the ink colors.


###2. What is an appropriate set of hypotheses for this task?
The null hypothesis is that there is no significant difference between the congruent sample (x<sub>1</sub>) and the incongruent sample (x<sub>2</sub>).
$$H_{0}: \bar{x}_{1} = \bar{x}_{2}$$
The alternative hypothesis is that there is a significant difference between the congruent sample and the incongruent sample.
$$H_{A}: \bar{x}_{1} \neq \bar{x}_{2}$$
####What kind of statistical test do you expect to perform? Justify your choices.
I expect the data to be normally distributed. If that is true then can I use a Welch's t-test to determine significance.

ref: https://en.wikipedia.org/wiki/Welch%27s_t_test


###Personal Stroop times
Now it’s your chance to try out the Stroop task for yourself. Go to this link, which has a Java-based applet for performing the Stroop task. Record the times that you received on the task (you do not need to submit your times to the site.) Now, download this dataset which contains results from a number of participants in the task. Each row of the dataset contains the performance for one participant, with the first number their results on the congruent task and the second number their performance on the incongruent task.

####Personal Results
    Congruent: 13.847
    Incongruent: 30.938

###3. Report some descriptive statistics regarding this dataset. Include at least one measure of central tendency and at least one measure of variability.


    # http://stackoverflow.com/questions/19410042/how-to-make-ipython-notebook-inline-matplotlib-graphics
    %matplotlib inline
    
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import probplot, ttest_ind
    
    np.set_printoptions(suppress=True)
    
    data = pd.read_csv('congruent-vs-incongruent.csv')
    congru = data['Congruent']
    incon = data['Incongruent']
    data.head(3)




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Congruent</th>
      <th>Incongruent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12.079</td>
      <td>19.278</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16.791</td>
      <td>18.741</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.564</td>
      <td>21.214</td>
    </tr>
  </tbody>
</table>
</div>




    # http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.describe.html
    
    data.describe() 




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Congruent</th>
      <th>Incongruent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24.000000</td>
      <td>24.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>14.051125</td>
      <td>22.015917</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3.559358</td>
      <td>4.797057</td>
    </tr>
    <tr>
      <th>min</th>
      <td>8.630000</td>
      <td>15.687000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>11.895250</td>
      <td>18.716750</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>14.356500</td>
      <td>21.017500</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>16.200750</td>
      <td>24.051500</td>
    </tr>
    <tr>
      <th>max</th>
      <td>22.328000</td>
      <td>35.255000</td>
    </tr>
  </tbody>
</table>
</div>



We can see that means are very close to two standard deviations apart. This is a good sign.
Note that the std returned is Normalized using n-1.

The median and mean are also relatively close for each sample, meaning there is a possiblity that they are normaly distributed.

However, their variance (s<sup>2</sup>) is different therefore Welche's t-test is more appropiate than the student t-test.

###4. Provide one or two visualizations that show the distribution of the sample data. Write one or two sentences noting what you observe about the plot or plots.


    # http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.hist.html
    # http://matplotlib.org/api/pyplot_api.html
    
    plt.figure()
    congru.hist(color=(0, 0.8, 0.8, 0.5), bins=12, range=(0,40), label='Incongruent', stacked=False)
    incon.hist(color=(0.8, 0.8, 0, 0.5), bins=12, range=(0,40), label='Congruent', stacked=False)
    plt.ylim((0,10))
    plt.legend(prop={'size': 10})
    plt.xlabel('Stroop task time in seconds')
    plt.ylabel('Frequency of occurence')
    plt.title('Histogram of Stroop Task times')
    plt.show()
    plt.close()
    print('note: green represents where the Congruent and Incongruent plots overlap')


![png](output_9_0.png)


    note: green represents where the Congruent and Incongruent plots overlap



    # http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.probplot.html
    fig = plt.figure()
    ax1 = fig.add_subplot(121)
    ax2 = fig.add_subplot(122)
    
    array = probplot(congru, plot=ax1)
    array = probplot(incon, plot=ax2)
    
    fig.suptitle('Q-Q Plot of Normality', fontsize=14)
    fig.tight_layout()
    fig.subplots_adjust(top=0.87)
    ax1.set_title('Congruent')
    ax2.set_title('Incongruent')
    plt.show()
    plt.close()


![png](output_10_0.png)


Based on the Histogram and QQ plots the sampling distributions of both samples appear normal.

###5. Now, perform the statistical test and report your results.


    # http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.ttest_ind.html
    degrees_of_freedom = congru.count() + incon.count()-2
    ttest_stat, ttest_p = ttest_ind(congru, incon, equal_var=False)
    print('degrees of freedom: '+str(degrees_of_freedom)+'\ntt statistic: '+str(ttest_stat)+'\ntp value: '+str(ttest_p))

    degrees of freedom: 46
    tt statistic: -6.5322505539
    tp value: 6.51016739044e-08


####What is your confidence level and your critical statistic value?
My confidence level is 99.9% with a critical value of ±3.551.

####Do you reject the null hypothesis or fail to reject it?
We reject the null hypothesis.

####Come to a conclusion in terms of the experiment task.
We conclude that the incongruent task takes longer than the congruent task.

####Did the results match up with your expectations?
These results mached my initial expectations of the data.

###6. Optional: What do you think is responsible for the effects observed? Can you think of an alternative or similar task that would result in a similar effect? Some research about the problem will be helpful for thinking about these two questions!

There is a trick we used to play on adults when I was a kid. We would tell them to say toast really quickly a whole bunch of times and then ask them what goes into a toaster. Without fail they would respond "TOAST!!!", then a few seconds later recant, saying "No, wait, I mean bread."

One possible explanation is that optics and linguistics are processed in different parts of the brain and since we are asking for a linguistic response to a optical question the linguistics centre has to wait .

Another possible and less complicated answer is that because the decision centre is receiving data from two places that contradict eachother it takes more time for it to figure out which one is right.

ref: https://en.wikipedia.org/wiki/Stroop_effect#Theories
