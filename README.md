import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

#%%
# read the data file into a dataframe
df = pd.read_csv(r'D:\Lectures\Industrial AI\machine_data-1.csv')
print(df)

print(df.shape)

#%%
if 'Unnamed: 0' in df.columns:
    df = df.drop(columns=['Unnamed: 0'])



#%%

#%%
"""
Extract data for a given manufacturer
"""
grpByManu = df.groupby(['manufacturef'])

dfa = grpByManu.get_group(('A',))

print(dfa)




#%%

loada = dfa['load']
timea = dfa['time']





#%%

'''
Is there a relationship between load and time
'''
plt.scatter(loada, timea)
plt.title("Relation between load and time")
plt.xlabel("Load")
plt.ylabel("Time")
plt.show()





#%%
'''
Characteristics of data
mean, median, mode
'''

# Calculate and assign mean, median, and mode for load
mode = dfa['load'].mode()[0]  # Get the mode
median = dfa['load'].median()  # Get the median
mean = dfa['load'].mean()  # Get the mean

# Print the values
print(f"Mode: {round(mode, 2)}")
print(f"Median: {round(median, 2)}")
print(f"Mean: {round(mean, 2)}")

# Relationship between mode, median, and mean
if mean == median == mode:
    print("\nData may follow a Normal distribution.")
elif mode < median < mean:
    print("\nData may follow an Exponential or Weibull distribution (β < 1).")
elif mode > median > mean:
    print("\nData may follow a left-skewed distribution (possibly Weibull with β > 1).")
else:
    print("\nThis is an unusual distribution.")


#%% 
# Define the new_func function to plot the histogram
def new_func(dfa):
    dfa[['load']].plot(kind='hist', bins=10, edgecolor='black')
    plt.title('Histogram of Load')
    plt.xlabel('Load')
    plt.ylabel('Frequency')
    plt.show()

# Call the new_func function
new_func(dfa)

#%%
variance = dfa['load'].var()  # Variance
std_dev = dfa['load'].std() # std

print(f"Variance: {round(variance, 2)}")
print(f"Standard Deviation: {round(std_dev, 2)}")

six_sigma_upper = mean + 6 * std_dev
six_sigma_lower = mean - 6 * std_dev
print(f"6 Sigma Range: ({round(six_sigma_lower, 2)}, {round(six_sigma_upper, 2)})")


#%%
