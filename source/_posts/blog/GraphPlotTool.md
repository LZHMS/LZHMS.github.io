---
title: A Graph Plot Tool that Integrates Data Processing And Visualization
date: 2024-01-31 23:15:27
toc: true
tags:
    - Data Visualization
categories: blog
excerpt: I developed a useful graph plot tool that integrates data processing and visualization by the format of python class.
---
### Project Envrionments
```python
# confiure the project environment
import pandas as pd
import numpy as np
from sklearn.preprocessing import Normalizer
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.font_manager import FontProperties
import warnings
warnings.filterwarnings('ignore')
```
### Overview Structure
```python
class GraphPlot:

    # Initialize the configuration of GraphPlot Tool
    def __init__(self, fname1="times.ttf", fname2="ARLRDBD.TTF"):
        # Set up Seaborn style
        sns.set(style="darkgrid")
        plt.rcParams['axes.unicode_minus'] = False
        # Fonts style
        self.Efont_prop1 = FontProperties(fname="C:/Windows/Fonts/" + fname1)
        self.Efont_prop2 = FontProperties(fname="C:/Windows/Fonts/" + fname2)
        # Set colors
        self.colors_name = ['yellow', 'blue', 'green', 'magenta', 'red', 'cyan', 'purple', 'orange', 'gray', 'pink']
        self.colors_rgb = ['#f94144', '#f3722c', '#f8961e', '#f9844a', '#f9c74f', '#90be6d', '#43aa8b', '#4d908e', '#577590', '#277da1']
        self.figsize = (10, 6)
        self.dpi = 400
        self.data = None


    def LoadData(self, data_path, sheet_name=0, sep=',', header=0, index_col=None):
        """LoadData Function
            Input: 
                data_path: the path of data file
                sheet_name: the table of Excel file
                sep: the separate sign of csv file
                header: the row of column names
                index_col: the index column of data file
            Output:
                self.data: the data read from given data file
            Function:
                From given source to reach the needed data.
        """
        if data_path.split('.')[-1] in ('xlsx', 'xls'):
            self.data = pd.read_excel(data_path, sheet_name=sheet_name, header=header, index_col=index_col)
        elif data_path.split('.')[-1] == 'csv':
            self.data = pd.read_csv(data_path, sep=sep, header=header, index_col=index_col)
        else:
            raise Exception('Unknown data file type!')
        self.PrintData()
    
    def DataProcessing(self, date_name=None, fill_name=None, fill_method="Nearest", standard_name=None, standard_method="Zscore"):
        """DataProcessing Function
            Input: 
                date_name: the column need to be converted to date style
                fill_name: the columns need to fill the missing values
                fill_method: the methods used to fill the missing values, alternatives like "Nearest"(default), "Linear", "Polynomial",
                             "Spline", "Mean", "Ffill", "Bfill", other specific value. 
                standard_name: the columns need to standardized
                standard_method: the methods used to standardize the columns, alternatives like "Zscore"(default), "Minmax".
            Output:
                self.data: the data has been transformed
            Function:
                Process the data got from dat file by filling the missing values and standardize some columns.
        """
        if date_name is not None:
            # Assuming the date_name column has be converted to the format like "2024-1-31"
            self.data[date_name] = pd.to_datetime(self.data[date_name])

        if fill_name is not None:
            if fill_method == "Nearest":
                self.data[fill_name] = self.data[fill_name].interpolate(method="nearest")
            elif fill_method == "Linear":
                self.data[fill_name] = self.data[fill_name].interpolate(method="linear")
            elif fill_method == "Polynomial":
                self.data[fill_name] = self.data[fill_name].interpolate(method="polynomial", order=2)
            elif fill_method == "Spline":
                self.data[fill_name] = self.data[fill_name].interpolate(method="spline", order=2)
            elif fill_method == "Mean":
                self.data[fill_name] = self.data[fill_name].fillna(self.data[fill_name].mean())
            elif fill_method == "Ffill":
                self.data[fill_name] = self.data[fill_name].fillna(method="ffill")
            elif fill_method == "Bfill":
                self.data[fill_name] = self.data[fill_name].fillna(method="bfill")
            else:
                self.data[fill_name] = self.data[fill_name].fillna(int(fill_method))
        
        if standard_name is not None:
            stdata = self.data[standard_name]
            if standard_method == "Zscore":
                self.data[standard_name] = (stdata - stdata.mean()) / stdata.std()
            elif standard_method == "Minmax":
                self.data[standard_name] = (stdata - stdata.min()) / (stdata.max() - stdata.min())
                
        self.PrintData()
        

    def PrintData(self):
        """PrintData Function
            Input: 
                None
            Output:
                None
            Function:
                Output the length of data, the first 5 elements and last 5 elements of data.
        """
        print('-'*30 + '\n' + 'Data Information:\n' + '-'*30)
        print("Data Length: %d" % (len(self.data)))
        print("Data Head:")
        print(self.data.head())
        print("Data Tail:")
        print(self.data.tail())
        print('-'*30)

    def LinePlot(self, column_X, column_Y, fontsize=8, color=7, font_prop=None, title=None):
        """LinePlot Function
            Input: 
                column_X: the column data as the x axis
                column_Y: the column data as the y axis
                fontsize: the main size of the font in the figure
                color: the color of plotted line
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a normal line figure and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        plt.plot(column_X, column_Y, color=self.colors_name[color])
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.savefig("images/LinePlot.svg", format="svg")

    def BiLinesPlot(self, column_X, column_Y1, column_Y2, fontsize=8, colors=[7, 6], labels=['Y1', 'Y2'], font_prop=None, title=None):
        """BiLinesPlot Function
            Input: 
                column_X: the column data as the x axis
                column_Y1: the column data as the y1 axis
                column_Y2: the column data as the y2 axis
                fontsize: the main size of the font in the figure
                colors: the colors list of the two data series 
                labels: the labels list of the two data series
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a two lines figure and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        plt.plot(column_X, column_Y1, color=self.colors_rgb[colors[0]], label=labels[0])
        plt.plot(column_X, column_Y2, color=self.colors_rgb[colors[1]], label=labels[1])
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.legend(prop=font_prop)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.savefig("images/BiLinesPlot.svg", format="svg")
    
    def MultiLinesPlot(self, column_X, columns_Y, fontsize=8, labels=None, font_prop=None, title=None):
        """MultiLinesPlot Function
            Input: 
                column_X: the column data as the x axis
                columns_Y: the columns data as the y axis must has the same length
                fontsize: the main size of the font in the figure
                labels: the labels list of the data series
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a multiple lines figure and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        labels = ['Y' + str(i+1) for i in range(columns_Y.shape[1])] if labels is None else labels
        for i in range(columns_Y.shape[1]):
            plt.plot(column_X, columns_Y[:, i], color=self.colors_rgb[i], label=labels[i])

        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.legend(prop=font_prop)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.savefig("images/MultiLinesPlot.svg", format="svg")


    def LineShadowPlot(self, column_X, column_Y, fontsize=8, color=7, alpha=0.3, font_prop=None, title=None):
        """LineShadowPlot Function
            Input: 
                column_X: the column data as the x axis
                column_Y: the column data as the y axis
                fontsize: the main size of the font in the figure
                color: the color of plotted line
                alpha: the parameter controls the transparency of the filled region
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a line figure with filled region and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        plt.plot(column_X, column_Y, color=self.colors_name[color])
        plt.fill_between(column_X, column_Y, color=self.colors_name[color], alpha=alpha)
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.tight_layout()
        plt.savefig("images/LineShadowPlot.svg", format="svg")

    def MultiLinesShadowPlot(self, column_X, columns_Y, fontsize=8, labels=None, alpha=0.3, font_prop=None, title=None):
        """MultiLinesShadowPlot Function
            Input: 
                column_X: the column data as the x axis
                columns_Y: the columns data as the y axis must has the same length
                fontsize: the main size of the font in the figure
                labels: the labels list of the data series
                alpha: the parameter controls the transparency of the filled region
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a multiple lines figure with filled region and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        labels = ['Y' + str(i+1) for i in range(columns_Y.shape[1])] if labels is None else labels

        for i in range(columns_Y.shape[1]):
            plt.plot(column_X, columns_Y[:, i], color=self.colors_rgb[i], label=labels[i])
            plt.fill_between(column_X, columns_Y[:, i], color=self.colors_rgb[i], alpha=alpha)

        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.legend(prop=font_prop)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.tight_layout()
        plt.savefig("images/MultiLinesShadowPlot.svg", format="svg")

    def NormPlot(self, column, bins=30, fontsize=8, colors=[7, 4], labels=['Data Distribution', 'Normal Distribution'], font_prop=None, title=None):
        """NormPlot Function
            Input:
                column: the column data to visualize the distribution
                bins: the number of bars in hist
                fontsize: the main size of the font in the figure
                font_prop: the main property of fonts
                colors: the first element set the color of hist graph, and the second element set the color of curve graph
                labels: the label of hist graph and the label of curve graph 
                title: the title of the figure
            Output:
                None
            Function:
                Plot a data distribution figure with the normal distribution curve and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        # Produce Norm Distribution Data
        mean, std = np.mean(column), np.std(column)
        x = np.linspace(mean - 3 * std, mean + 3 * std, 100)
        y = (1 / (np.sqrt(2 * np.pi) * std)) * np.exp(-0.5 * ((x - mean) / std) ** 2)
        
        # Plot Data Distribution
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        plt.hist(column, bins=bins, density=True, alpha=0.7, color=self.colors_rgb[colors[0]], label=labels[0])
        plt.plot(x, y, color=self.colors_name[colors[1]], label=labels[1])
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.legend(prop=font_prop)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.savefig("images/NormPlot.svg", format="svg")

    def ACFPlot(self, column, lags, y_bound=[-1, 1], fontsize=8, font_prop=None, title=None):
        """ACFPlot Function
            Input:
                column: the column data to visualize the distribution
                lags: the number of lags
                y_bound: the lower bound and upper bound of the y axis
                fontsize: the main size of fonts
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a ACF figure of the data and save the image as the format of SVG.
        """
        from statsmodels.graphics.tsaplots import plot_acf

        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        _, ax = plt.subplots(facecolor='white', figsize=self.figsize, dpi=self.dpi)
        plot_acf(column, lags=lags, ax=ax)
        ax.set_ylim(y_bound)
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.savefig("images/ACFPlot.svg", format="svg")
    
    def HeatMapPlot(self, columns, fontsize=8, font_prop=None, title=None):
        """HeatMapPlot Function
            Input:
                columns: the columns data need to calculate the correlation matrix 
                fontsize: the main size of fonts
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a heat map figure of the data and save the image as the format of SVG.
        """
        correlation_matrix = columns.corr()

        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        font_format = {'fontsize': 10, 'fontweight': 'bold', 'color': 'black'}
        plt.figure(figsize=self.figsize, dpi=self.dpi)
        sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt=".2f", annot_kws=font_format, cbar=False)
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.tight_layout()
        plt.savefig("images/HeatMapPlot.svg", format="svg")

    def ScatterMatrixPlot(self, columns, bins=40, colors=[(0.3, 0.3, 0.3), (0.5, 0.5, 0.5)], alphas=[0.7, 0.7], fontsize=8, font_prop=None, title=None):
        """ScatterMatrixPlot Function
            Input:
                columns: the columns data need to calculate the correlation matrix 
                bins: the number bars of the hist graph
                colors: the edge color of the hist and the point color of the scatter graph
                alphas: the transparency of the edge in hist and point in scatter graph
                fontsize: the main size of fonts
                font_prop: the main property of fonts
                title: the title of the figure
            Output:
                None
            Function:
                Plot a scatter matrix figure of the data and save the image as the format of SVG.
        """
        font_prop = self.Efont_prop2 if font_prop is None else font_prop
        variables = [columns.iloc[:, i] for i in range(columns.shape[1])]

        # Create a scatter matrix plot
        _, axes = plt.subplots(nrows=columns.shape[1], ncols=columns.shape[1], figsize=self.figsize, dpi=self.dpi)
        # Plot scatter plots for each pair of variables
        for i in range(columns.shape[1]):
            for j in range(columns.shape[1]):
                if i == j:
                    # If on the diagonal, create a histogram
                    axes[i, j].hist(variables[i], bins=bins, color=self.colors_rgb[i], edgecolor=colors[0], alpha=alphas[0])
                else:
                    # Otherwise, create a scatter plot
                    axes[i, j].scatter(variables[j], variables[i], alpha=alphas[1], s=4, c=colors[1])
        
        plt.yticks(fontproperties=font_prop, fontsize=fontsize)
        plt.xticks(fontproperties=font_prop, fontsize=fontsize)
        plt.title(title, fontproperties=font_prop, fontsize=fontsize + 4)
        plt.tight_layout()
        plt.savefig("images/ScatterMatrixPlot.svg", format="svg")
```

### Contributors
- [Zhihao Li](https://lzhms.github.io/)