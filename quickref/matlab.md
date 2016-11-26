---
layout: post
title: Quick Reference of Matlab
mathjax: false
related: false
comments: false
published: true
---

_Last modified: Jun, 2014_

## A typical plotting script in Matlab

{% highlight matlab %}
x = linspace(0,2*pi, 100);
y1 = sin(x); 
y2 = cos(x);

h_fig = figure(1);
set(h_fig, 'Position', [0, 0, 600, 500])
plot(x, y1, '-', x, y2, '--')
xlabel('x', 'fontsize', 16')
ylabel('y', 'fontsize', 16)
set(gca, 'fontsize', 14, 'xlim', [0, 2*pi], 'ylim', [-2, 2],'ytick', [-2:0.5:2]) % set axis properties
h_lines = findobj(gca, 'type', 'line'); % tweaking the line properties
set(h_lines(1), 'linewidth', 4, 'color', 'b')  % h_lines(1) is the last curve in the being plotted (y2 in this case)
set(h_lines(2), 'linewidth', 2, 'color', 'r')  % should change the y1 curve properties
h_leg = legend('sin(x)', 'cos(x)');
set(h_leg, 'fontsize', 16)
legend boxoff
text(1.2, 1.2, 'sin(x)', 'fontsize', 20, 'color', 'b')
text(3, -1.2, 'cos(x)', 'fontsize', 20, 'color', 'r')
{% endhighlight %}

## Font size unchangeable in Unix/Linux

It happened to my fresh installed Xubuntu 11.10: I could not change the font type and size in a figure. This might have something to do with Matlab itself --- the fonts in figures were rendered by the old X while fonts outside were done with Java. To overcome this problem, one possible solution was to install necessary DPI fonts:

```
sudo apt-get install xfonts-100dpi xfonts-75dpi
```

__Reference__: 

* http://ubuntuforums.org/showthread.php?t=1762805
* http://ifixdit.blogspot.com/2011/08/how-to-fix-matlab-small-figures-and.html

(Installation of 'xfonts-100dpi' and 'xfonts-75dpi' also solved the default messy fonts problem of Google Earth in Kubuntu/Xubuntu.)



## Import multiple data files into MATLAB workspace

{% highlight matlab %}
files = dir('*.txt');
for i=1:length(files)
eval(['load ' files(i).name ' -ascii']);
end
{% endhighlight %}

NOTE: One can also use "_importdata_" function in MATLAB. 

__Reference__: 

* http://www.mathworks.com/support/solutions/en/data/1-190XP/index.html?solution=1-190XP


## How to remove axes in MATLAB plot

One can use the "'Visible'" axis property to remove **ALL** axes, including tick marks and labels: 

{% highlight matlab %}
set(gca,'Visible','off')
{% endhighlight %}

A quick and dirty way to get rid of just one axis: remove all tick marks and then set its color to white (or whatever the current background colour may be:

{% highlight matlab %}
set(gca,'YTick',[])
set(gca,'YColor','w')
{% endhighlight %}


##  Matlab 2011b starting error: "libc.so.6" not found

The error usually goes like this in a fresh installed Matlab 2011b on Ubuntu

```
/matlab/bin/util/oscheck.sh: 605: /lib64/libc.so.6: not found
```

The solution to this problem: 

For __Ubuntu 12.04 64-bit__, the fix is 

```
sudo ln -s /lib/x86_64-linux-gnu/libc.so.6 /lib64/libc.so.6
```

Other Linux distros can adapt similar fix. For example, for 32-bit Linux:

```
sudo ln -s /lib/i386-linux-gnu/libc-2.13.so /lib/libc.so.6
```

For 64-bit:
```
sudo ln -s /lib64/x86_64-linux-gnu/libc-2.13.so /lib64/libc.so.6
```


## Library missing when starting external program

It is handy to access external programs in MATLAB. But sometimes libraries are reported missing. For example, I was trying to compile a FORTRAN script with _gfortran_ in my 64-bit Ubuntu system 

{% highlight matlab %}
system('gfortran myscript.for')
{% endhighlight %}

and MATLAB complained 

```
/home/user/MATLAB/R2011b/sys/os/glnxa64/libgfortran.so.3: version `GFORTRAN_1.4' not found
```

The fix is to symbolically link system's libraries to MATLAB's corresponding directory. 

```
cd /home/user/MATLAB/R2011b/sys/os/glnxa64
mv libgfortran.so.3 libgfortran.so.3_bak
ln -s /usr/lib/x86_64-linux-gnu/libgfortran.so.3 
```


## Function parameter passing 

For example, if one needs to solve an equation 'sin(x)=a*x+b' within the main function, while 'a' and 'b' are calculated previously in the main function. In this case, the main function needs to pass the parameters 'a' and 'b' to the equation. 

One can create another function 'func2.m' in which

{% highlight matlab %}
y = func2(x, a, b)
y = sin(x)-a*x-b
end
{% endhighlight %}

And in the main function, solve this equation:

{% highlight matlab %}
sol = fsolve(@(x)func2(x,a,b), 0);
{% endhighlight %}


## Automatic legend generation with an array

Sometimes I need to add automatically generated legend to the plot. For example, I have an array 

{% highlight matlab %}
arr = 1:1:5; 
{% endhighlight %}

And I need to make the values in variable "'arr'" as the legend entries. That is, I have five curves, and each curve corresponds to "a=1", "a=2", "a=3", "a=4" and "a=5" in legend. One way to automate this legend generation is

{% highlight matlab %}
leg_str = cell(1, length(arr)); 
% It should be a cell in MATLAB rather an array

for ii = 1:length(arr)
    leg_str(ii) = {strcat('a=', num2str(arr(ii)))};
end

leg = legend(leg_str, 'fontsize', 16);
{% endhighlight %}

Note that the variable "leg_str" is a **cell**, not an array (It seems "legend.m" deals strings in cell format). 


## Regular expression in Matlab

Take this example string

{% highlight matlab %}
test_str = 'if you are going to San Francisco'
{% endhighlight %}

* Replace str: use function regexprep (regular expression with replacement), like

```
>> regexprep(test_str, 'San Francisco', 'Boston')
ans = 
'if you are going to Boston'
```

## How to show partial legend in Matlab

http://www.mathworks.com/matlabcentral/answers/25381-how-to-show-partial-legend-in-figure

## How to remove repeating elements in a Matlab array

http://www.mathworks.com/matlabcentral/answers/16667-how-to-remove-repeating-elements-from-an-array

## How to get complementary subset of an array in Matlab 

http://stackoverflow.com/questions/15381188/complement-subset-in-matlab