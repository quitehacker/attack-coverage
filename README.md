# attack-coverage
An *excel*-centric approach for managing the MITRE ATT&amp;CK® tactics and techniques. 

## the goal

The Excel file *AttackCoverage.xlsx* can be used to get a *coverage measure* of MITRE ATT&amp;CK® tactics and techniques, in terms of *detections rules*. Working as DFIR consultants for different companies, with different SOCs and technologies in place, it was needed a *simple* and *portable* way to get a sort of *awareness* about which attackers' tactics/techniques a customer is able to detect and, more important, what it's missing.

## AttackCoverage.xlsx

Before a brief explanation about the usage, please consider that all the 7 worksheet share specific characteristics. The *header* of each worksheet has colours: *gray* means a static fields, strings or numbers; *blue* means calculated values, with formulas; *white* means columns (cells) that expect an input from the users. Usually you will not mess with gray or blue columns, with exceptions. *White* columns with "Active" or "IsActive" captions expect to be blank or filled with the string "*yes*": there is not a "*no*", just "*yes*" or blank.

## adding the first *detection rule*

From the *blue team* perspective, the great part of the job will be done in the **detections** worksheet. Here is where you'll set your detection rules: the provided worksheet has the first four columns as an example, but you can add/remove/change them. Unless you're modifying the excel, do not touch the "*is active*" and "*attack1..3*" columns. Let's insert the **first** detection rule, which aims to detect attackers' attempt to access the LSASS Memory, sub-technique T1003.001.

![adding detection rule](/images/ac_img_1.png)

The first columns, the *gray* ones, are up to you. If you want to make the detection rule *active*, simply write *yes* in the column. To **map** that specific rule to one or more (could be) Attack Techniques/Sub-Techniques, just use the *attack1..3* columns.

Let's switch on **techniques** worksheet. As you will see, we have two **red** lines: one for T1003.001 (LSASS Memory sub-technique) and one for T1003, the *technique* which T1003.001 belongs to.

![techniques](/images/ac_img_2.png)

The **red** colouring reflects the *inconsistent* state reported in column *technique status*. It means you have a detection rule for a specific (sub)technique but your're **missing any data source** required to detect it: check the column *data source available*, which is zero. Techniques data sources are written in the *data sources* column, separated by a pipe '|'. To "solve" the issue you can: disable the rule since it can't work; fix the missing data source as shown in the next picture, by accessing the **source** worksheet and putting "*yes*" in the proper field.

![sources](/images/ac_img_3.png)

## the **techniques** worksheet

Going back to the *techniques* worksheet you'll get two *green* lines and multiple *yellow* ones. First, the green ones: since you have the proper (or, better, *a* proper) data source for the detection rule, the technique status changed to **detect**. It means you *could* be able to detect that specific sub-technique T1003.001. Moreover, since you're detecting a sub-technique, the "*father*" technique T1003 will reflect this detection too, in a slightly different way. The "*default counting rule*" follows:

> By default the **minimum number of expected detection rules** is **1** for Techniques without any sub-technique. For Techniques with one or more sub-techniques, the minum number of expected detection rules is **the number of sub-techniques**. This number is automatically calculated and reported in the "*minimum detection rules*" column.

In the current scenario, the minimum expected detection rule for T1003.001 is *1*, while for T1003 (the Technique) is *8* because "OS Credential Dumping" has eight sub-techniques. What about the *yellow* lines ("*technique status*" equals to "*no detect*")? Since you've enabled a *data source*, any technique using that data source **could be detected**: in different words, you have the data to detect those techniques but *no detections rules* in place! Time to fill the gaps!

![techniques](/images/ac_img_4.png)

## the **STATUS** and the **COVERAGE** worksheets

What you are currently detecting in terms of techniques and sub-techniques, organized by *tactics*, is shown into the STATUS worksheet. It's a better view of the work done, what you're missing entirely (no data sources available!) and what you could detect if you'll prepare the proper detection rules.

![STATUS](/images/ac_img_5.png)

"*Wait a moment. Why in the STATUS cells related to T1003 and T1003.001 we have 0 detection rules and 1 detection rules? And both are green?*". Remember that the STATUS worksheet represents what you are detecting (techniques and sub-techniques) and what you are not. For the *coverage* there is the COVERAGE worksheet. As shown in the next picture, the COVERAGE will report *13%* for the Technique, since you have just 1 out of 8 detection rules expected for T1003.

![COVERAGE](/images/ac_img_6.png)

You'll spot that COVERAGE will address only Techniques organized in the "*classic*" Attack way, by Tactics. In the end, for each Tactics, you'll get the total coverage.

### *hey, I have a new fancy sub-techniques not included in the Attack framework*

This is supercool, and the Excel file is already built to cover that. Place the detection rule by using the **detection** worksheet and assign to the "*OS Credential Dumping (T1003)*" technique, since it will not apply to any of the sub-techniques described by the Attack framework.

![detections](/images/ac_img_7.png)

Go back to **techniques**: now you got **2** detection rules for T1003, one from T1003.001 and one directly applied to T1003. Unfortunately this is **unexpected**: techniques with sub-techniques are not expected to have detection rules direclty applied to them! This **error** is reported in the "*Error checks*" column.

![techniques](/images/ac_img_8.png)

> You can't have more detection rules than the expected ones! Coverage will be wrong. Remember the minimum expected ones: **1** for Techniques without any sub-technique; **n** for Techniques with *n* sub-techniques **and 0** for the Technique itself.

How to handle that? Easy, that's the reason of the *white* column "**detection rules modifier**". Just add *1* to the T1003 related cell: it means we *expect* a detection rule that will *direclty* target the "main" Technique T1003. See the pictures.

![techniques](/images/ac_img_9.png)

No more errors: STATUS and COVERAGE will reflect this new addendum

![STATUS](/images/ac_img_10.png)

![COVERAGE](/images/ac_img_11.png)



## how is built


## credits
