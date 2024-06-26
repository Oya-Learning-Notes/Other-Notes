In this post we are going to introduce two different ways to calculate the **maximum ascending sub array** for an array.

# What Is Maximum Ascending Sub Array?

For array: `1 3 4 2 3 6 5 9`, one of it's MSA is `1 2 3 5 9`, of course there are several different MSA for this array. For serious definition of MAS, please go to Google lol.

Usually, the question will ask the size of the max ascending sub array, so the following examples will focus on calculating the sub array size, but actually once you get the point of the algorithm, you will also know how to output the sub array as the result.

# Dynamic Programming

This is the one of the most general algorithm to get max ascending sub array.

For an array `arr[n]`, we create a new array `subAns[n]`.

`subAns[i] = k` means the answer (that is, the size of the MSA) of the sub array from `arr[0]` to `arr[i]`. For example, `subAns[5] = 2` means if we only consider first 5 elements in `arr`, then the size of MSA is `2`.

![IMG_2100](https://github.com/Oya-Learning-Notes/OOP-Learning-Note/assets/61616918/474b2ef1-f5be-42b9-ac48-f29fe96fb668)

Now question becomes: if we know `subAns[0:i-1]`, how to calculate `subAns[i]`.

Actually that's quite simple. The answer is:

1. Read value of `arr[i]`, find all indexs of elements in `arr[0:i-1]` that less than `arr[i]`.
2. For all indexs obtain from the step above, calculate `subAns[index] + 1`.
3. Choose the max result from step above, and that's the final result of `subAns[i]`.

Check out the example image below, which will show how to calculate `subAns[6]` for the example array.

![IMG_2101](https://github.com/Oya-Learning-Notes/OOP-Learning-Note/assets/61616918/8bc1a2c6-5e18-4e8e-b784-fd07b774f3a1)

The final answer for `arr[6]` is 4.

After knowing this, all the things we need to do is loop to calculate from `subAns[0]` to `subAns[n]` which is also the final answer that we want.

## Time Complexity

Since we need to scan the whole `subAns` everytime we add new element, the total time complexity should be $O(n^2)$ .

# Grouping

This is a completely new way to calculate the MSA.

The core thought is to grouping the elements into several groups, and in the end the **max size of MSA would be the total number of the groups**.

## Steps

Let's see how to do it. The basic step is as follows:

- Loop `arr` from begin to end, take one element out at a time.
- Check the already existing groups from begin to end
  - If this element is less than the last element in that group, put this element into that group.
  - If this element is greater than the last element in all existing groups, create a new group and add this element into it.

When finishing the steps, the **size of the MSA would be the number of the groups**.

# How grouping Algorithm Work?

Let's use the image below as our example:

![image](https://github.com/Oya-Learning-Notes/Note-Assets/assets/61616918/6dbb874e-f6b5-4992-a43a-dda5682a7d6f)

In this picture, we have already finished the grouping step. But **why the number of the group would equal to the size of MSA**?

## Properties of groups

Here we should know some properties of the groups and these elements:

1. Elements in a groups is in desending order.
2. The order in which the elements appears in a groups is coincide with the order they appears in `arr`.
3. If we take the last elements from all groups, then they are in ascending order.

Property 1 and property 2 is quite clear. We specify only smaller element could add to the end of the group, so it must be descending in the end. Also we deal with the elements one by one from beginning to end, so the order is coincide.

Property 3 is relatively more indirect, but it's also easy to prove. We could use the idea of counterfactuals, just consider in this steps we could get a non-ascending last elements sequence, and at the end we will find it conflict with the steps speficication.

![image](https://github.com/Oya-Learning-Notes/Note-Assets/assets/61616918/b45dc688-19fe-47cf-a2f8-f8a323a39595)

Let's see how to make the situation above happen.

We divide this into 2 sub-situation, one is element 3 arrives first, another is 4 arrives first.

![image](https://github.com/Oya-Learning-Notes/Note-Assets/assets/61616918/9198aff4-123d-4eb9-9770-3e3414c06710)

- If 4 arrives first, then 3 will not at group(n+k) but at least goes to g(n), because 4 > 3.
- If 3 arrives first, and 4 goes g(n), means A > 4 > 3, so 3 should have been at g(n) when it arrived first.

Both sub-situtation is conflict.

Now let's see what we could deduce using these properties.

## First Conclusion

**There will be at most 1 elements in the final MSA in one group**. This means choice any element from a group will prevent any other element in that group to also appear in the final MSA.

![image](https://github.com/Oya-Learning-Notes/Note-Assets/assets/61616918/3c495537-36ab-4e17-9aa7-1c7e957c5afb)

This is because really easy to understand because we already knows:

- the order in which elements appears in one group will coincide with the order they were in the original array.
- Elements in a groups is in desending order.

Because that MSA requires ascending elements, so **choosing one element from a group means you can NOT choose all elements in this group after the one you already chosen in the group anymore**. because they were all less than the one you choose and they will appear after the one you choose.  Also **you can NOT choose the elements before this one** because, similarly, they all greater than the one you choose and will all appear before the one you choose.

> Note that **this does NOT means that choosing elements in a group will not affect the choose-ability of the elements in other group**, in fact in will. For example in the image above, choosing `9` in `g4` will prevent us from choosing `4` from `g3`.

## Second Conclusion

**We could always find a way to choose an elements from each groups which were not conflict and could form the ascending array.**

As we talk above, choosing one element from a group will have the probability to make element in other group unchooseable. So why we say there must be a way to choose out one element from each group?

Let's think a question, when adding a new elements into a group, can we know which elements this new element will NOT affect? Luckily, we actually know!

That is, **when we confirmed that an element will be added to `g[k]`, then when we choose this elements, we could always choose the last element of `g[k-1]`** without breaking the rules of ascending array.

This is because:

- The last element of g[k-1] must appears before newly added element in `arr`.
- Ths last element of g[k-1] must less than newly added element.

Let's do the grouping work again, but this time we use arrows to point out the not-affected element relationship.

![image](https://github.com/Oya-Learning-Notes/Note-Assets/assets/61616918/c7453e62-5b6c-451d-a763-026e923666b1)

Notice that the pointing work is done simultaneously when we grouping elements, not after finishing all grouping works.

Now we know each elements will have at least one non-affected arrow which point to a certain element in the group directly before the group it in. All we need to do is to start from an element at the last group, choosing the elements following the instruction of these arrows, then we will get an MSA.

![image](https://github.com/Oya-Learning-Notes/Note-Assets/assets/61616918/1a3a975b-f662-4e67-bad6-ec8ece140e91)

-----

Now we know:

- **At most one element** of a group could be in the MSA.
- **At least there will be a way** to choose an element from each group to from ascending sub array.

From first conclusion we know, if we want to make the ascending array as long as possible, then the maximum would be the group number. From the second conclusion we know, it's always possible to choose one elements from each group to form ascending sub array.

So that's it. We could always find a MSA with the length equal to the number of the groups.

> All the _less than_ in this passage could be exchanged with _less than or equal to_ statements, it's acutally decided by what you want, if you want a strictly ascending sub array, then you should use _less than or equal to_ , otherwise just use _less than_ will be enough.

## Time Complexity

If we use traditional looping to find where group a new element should be added to, then the complexity is the same to the DP method which is $O(n^2)$ .

But if we use _binary search_ to find the place, then time complexity will reduce to $O(n \cdot log n)$ .

> Because we have proofed that the last element of the groups is ascending, so we could implement binary search to find place for new element.


## Code Example

Anyway this part is alternative, if you don't already know how the algorithm works and don't want to know the specific implementation, then just skip this part.

Here I make an code implementation/example using C++.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using std::cout, std::endl;
using std::vector;

using LL = long long;

class GroupItem
{
public:
    LL mArrIndex;
    LL mValue;
    LL mNotAffectIndex;

    GroupItem(LL arrIndex, LL value, LL notAffectIndex)
        : mArrIndex(arrIndex),
          mValue(value),
          mNotAffectIndex(notAffectIndex) {}
};

class Group
{
public:
    LL mGroupIndex;
    vector<GroupItem> mItems;

    Group(LL groupIndex, GroupItem firstItem)
        : mGroupIndex(groupIndex)
    {
        mItems.clear();
        mItems.push_back(firstItem);
    }

    LL last() const
    {
        return (*(mItems.end() - 1)).mValue;
    }

    friend std::ostream &operator<<(std::ostream &os, const Group &ins);
};

std::ostream &operator<<(std::ostream &os, const Group &ins)
{
    os << "g[" << ins.mGroupIndex << "]: " << endl;
    for (const GroupItem &item : ins.mItems)
    {
        os << "arr[" << item.mArrIndex << "]: " << item.mValue
           << "      (Non-affected -> g[" << ins.mGroupIndex - 1 << "][" << item.mNotAffectIndex << "])" << endl;
    }
    return os;
}

// Find the group index in which where the new element should be
//
// If thie element could not be put into any currently existing group, return -1
// (Which means in which circumstance you need to create a new group)
LL findInsertPlace(const vector<Group> &groupList, const LL &element)
{
    // get group count, init left, right, mid
    LL groupCount = groupList.size();
    LL left = 0;
    LL right = groupCount - 1;
    LL mid = 0;

    // if need to create new group
    if (groupCount < 1 || groupList[groupCount - 1].last() <= element)
    {
        return -1;
    }

    // start binary search
    while (left < right)
    {
        LL mid = (left + right) / 2;
        LL midGroupLast = groupList[mid].last();

        if (midGroupLast > element)
        {
            right = mid;
        }
        else if (midGroupLast <= element)
        {
            left = mid + 1;
        }
    }

    return left;
}

void addToGroupList(vector<Group> &groupList, LL elementIndex, LL element)
{
    LL index = findInsertPlace(groupList, element);

    // add new group
    if (index == -1)
    {
        LL notAffected = -1;
        if (groupList.size() > 0)
        {
            notAffected = groupList[groupList.size() - 1].mItems.size() - 1;
        }
        groupList.push_back(
            Group(
                groupList.size(),
                GroupItem(
                    elementIndex,
                    element,
                    notAffected)));

        return;
    }

    LL notAffected = -1;
    if (index > 0)
    {
        notAffected = groupList[index - 1].mItems.size() - 1;
    }

    // add to existing group
    groupList[index].mItems.push_back(
        GroupItem(
            elementIndex,
            element,
            notAffected));
}

int main()
{
    // create test arr
    vector<LL> arr{1, 2, 4, 2, 3, 6, 5, 9, 4};
    LL arrSize = arr.size();
    vector<Group> groupList;
    for (LL i = 0; i < arrSize; ++i)
    {
        addToGroupList(groupList, i, arr[i]);

        // show groups info
        cout << "-----------------------after adding arr[" << i << "]: " << arr[i] << "------------------------" << endl;
        for (const Group &group : groupList)
        {
            cout << group << endl;
        }
    }

    return 0;
}
```

Noticed that I used _binary search_ in this code example. And I have print the very detailed info of the elements inside the groups after adding each elements so you can clearly see how it works.

The output is:

```
-----------------------after adding arr[0]: 1------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

-----------------------after adding arr[1]: 2------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

-----------------------after adding arr[2]: 4------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])

-----------------------after adding arr[3]: 2------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])
arr[3]: 2      (Non-affected -> g[1][0])

-----------------------after adding arr[4]: 3------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])
arr[3]: 2      (Non-affected -> g[1][0])

g[3]:
arr[4]: 3      (Non-affected -> g[2][1])

-----------------------after adding arr[5]: 6------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])
arr[3]: 2      (Non-affected -> g[1][0])

g[3]:
arr[4]: 3      (Non-affected -> g[2][1])

g[4]:
arr[5]: 6      (Non-affected -> g[3][0])

-----------------------after adding arr[6]: 5------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]: 
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])
arr[3]: 2      (Non-affected -> g[1][0])

g[3]:
arr[4]: 3      (Non-affected -> g[2][1])

g[4]:
arr[5]: 6      (Non-affected -> g[3][0])
arr[6]: 5      (Non-affected -> g[3][0])

-----------------------after adding arr[7]: 9------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])
arr[3]: 2      (Non-affected -> g[1][0])

g[3]:
arr[4]: 3      (Non-affected -> g[2][1])

g[4]:
arr[5]: 6      (Non-affected -> g[3][0])
arr[6]: 5      (Non-affected -> g[3][0])

g[5]:
arr[7]: 9      (Non-affected -> g[4][1])

-----------------------after adding arr[8]: 4------------------------
g[0]:
arr[0]: 1      (Non-affected -> g[-1][-1])

g[1]:
arr[1]: 2      (Non-affected -> g[0][0])

g[2]:
arr[2]: 4      (Non-affected -> g[1][0])
arr[3]: 2      (Non-affected -> g[1][0])

g[3]:
arr[4]: 3      (Non-affected -> g[2][1])

g[4]:
arr[5]: 6      (Non-affected -> g[3][0])
arr[6]: 5      (Non-affected -> g[3][0])
arr[8]: 4      (Non-affected -> g[3][0])

g[5]:
arr[7]: 9      (Non-affected -> g[4][1])
```

Some explanation:

- `Non-affected -> g[i][j]`  means this element's non-affected element is in `g[i]`, and it's the `g[i].mItems[j]`. More clearly, `i` spefify the group number, and `j` specify the position of the element in that group.
- `g[-1][-1]` means this elements is in the first group and have no concept of non-affected element.
