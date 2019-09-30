---
layout: post
title: 多点在矩形中均匀随机排列算法的实现
create_date: 2012-11-28
modify_date: 2019-09-30
excerpt: 将多个不规则图片随机的放在一个给定矩形中并且使他们排列相对均匀不要太难看即可。
--- 

不规则图形在一个固定矩形中的排列很容易让人联想到背包问题。OMG这太复杂了，所以我们不研究这个。我的问题很简单，将多个不规则图片随机的放在一个给定矩形中并且使他们排列相对均匀不要太难看即可。Google一番后，找到[这么一个](https://home.comcast.net/~davejanelle/packing.html)算法基本可以满足我的需求。

**算法描述：**

1. 将N个点随机的放到矩形中
2. 定义一个步长STEP。可先设置为矩形某边长的1/5。（我实际使用的是Max(边长)）
3. 对每一个点P:
    1. 向上下左右四个方向分别移动STEP的距离。
    2. 判断新的位置是否加大了点P与其最近邻点的距离？如果是则保留新位置。
4. 重复步骤3直到N个点中没有可以再移动的点。
5. 缩短STEP（比如除以1.5），再次重复步骤3直到STEP小于某个最小值。

**算法实现（objective-c + cocos2d）:**

写的比较烂，将就看吧...

+ (void) randomLayout:(NSMutableArray *)animals
{
    // given a rectangle
    float left = 0.0f + 32.0f;
    float top = 30.0f + 32.0f;
    float width = 320.0f - 64.0f;
    float height = 330.0f - 64.0f;

    // N points
    int N = [animals count];

    // 1) Randomly position N points inside a rectangle
    int i;
    float x, y;
    Animal * animal;

    for (i = 0; i &lt; N; i++)
    {
        animal = [animals objectAtIndex:i];
        x = arc4random() % (int)width + left;
        y = arc4random() % (int)height + top;
        animal.nextPos = ccp(x, y);
    }

    // 2) Define a step size. I start with 1/5 (square side length).
    float step = MAX(width, height);

    // 5) Make STEP smaller (I divide it by 1.5), and goto step 3 unless STEP is smaller than some minimum value.
    float nearestDistance;
    float newNearestDistance;
    float maxDistance;
    CGPoint testPosition, mdPosition;
    BOOL isAnyoneMoved;

    while (step &gt;= 1.0f)
    {
        //NSLog(@"==========step %f==========", step);
        // 4) Repeat step 3 until none of the N points is able to move.
        do
        {
            isAnyoneMoved = NO;

            // 3) For each of the N points:
            for (i = 0; i &lt; N; i++)
            {
                //NSLog(@"---point %d---", i);
                animal = [animals objectAtIndex:i];

                nearestDistance = [GameUtil getNearestAnimalDistance:animal.nextPos animals:animals];
                newNearestDistance = maxDistance = 0;

                // 3a) Perturb the location of the point by STEP in the North, South, East, and West directions.
                // 3b) Does this new location increase the distance to the nearest neighbor for this point? If so, keep the new location

                if (animal.nextPos.y + step &lt; top + height) // up
                {
                    testPosition = ccp(animal.nextPos.x, animal.nextPos.y + step);
                    newNearestDistance = [GameUtil getNearestAnimalDistance:testPosition animals:animals];
                }

                if (maxDistance &lt; newNearestDistance)
                {
                    maxDistance = newNearestDistance;
                    mdPosition = testPosition;
                }

                if (animal.nextPos.y - step &gt; top) // down
                {
                    testPosition = ccp(animal.nextPos.x, animal.nextPos.y - step);
                    newNearestDistance = [GameUtil getNearestAnimalDistance:testPosition animals:animals];
                }

                if (maxDistance &lt; newNearestDistance)
                {
                    maxDistance = newNearestDistance;
                    mdPosition = testPosition;
                }

                if (animal.nextPos.x + step &lt; left + width) // right
                {
                    testPosition = ccp(animal.nextPos.x + step, animal.nextPos.y);
                    newNearestDistance = [GameUtil getNearestAnimalDistance:testPosition animals:animals];
                }

                if (maxDistance &lt; newNearestDistance)
                {
                    maxDistance = newNearestDistance;
                    mdPosition = testPosition;
                }

                if (animal.nextPos.x - step &gt; left) // left
                {
                    testPosition = ccp(animal.nextPos.x - step, animal.nextPos.y);
                    newNearestDistance = [GameUtil getNearestAnimalDistance:testPosition animals:animals];
                }

                if (maxDistance &lt; newNearestDistance)
                {
                    maxDistance = newNearestDistance;
                    mdPosition = testPosition;
                }

                //NSLog(@"nd: %f, md: %f", nearestDistance, maxDistance);
                if (nearestDistance &lt; maxDistance)
                {
                    animal.nextPos = mdPosition;
                    isAnyoneMoved = YES;
                }
            }
        } while (isAnyoneMoved == YES);

        step *= 0.9f;
    }
}

+ (float) getNearestAnimalDistance:(CGPoint)position animals:(NSMutableArray *)animals
{
    Animal * neighbour;
    float distance;
    float ndistance = 99999.0f;

    for (neighbour in animals)
    {
        if (CGPointEqualToPoint(neighbour.nextPos, position)) {
            continue;
        }

        distance = ccpDistance(position, neighbour.nextPos);

        if (ndistance &gt; distance) {
            ndistance = distance;
        }
    }

    return ndistance;
}
