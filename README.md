# Chicago Crimes
### Background
(Hypothetical)
The City of Chicago's Police Department has been struggling with community support in recent years. Between high-profile officer-involved shootings nationwide and a perception that Chicago police are doing too little to curb the increase in gun violence in many neighborhoods, distrust of police may be worse than it's ever been. \[this part is probably real]
\[the rest is not real] The city has been looking into new ways to change the communities' perceptions of their police department. Through focus groups with community leaders, the City of Chicago learned that many civilians distrust the police because they are seen as "forces of destruction," even when they are sent in to help. Arrests can have a damaging impact on the offenders' families, and little is done to help the victims of the crimes when police are sent out. In order to combat negative perceptions of its police force, the City of Chicago has trained some of its police officers to act as "Community Support Officers": they act as police officers *and* social workers. Instead of focusing solely on the offenders, these officers are trained to assess the needs of the victims and the offenders' families and address them in whatever way they can. They are trained to offer brief emotional interventions and are aware of all the resources available from the City to help victims and families.
Because the Community Support Officer program is still in its infancy, the City of Chicago wants to focus on areas where they can make the most difference. In cases of domestic violence, the victim is typically *also* the offender's family. These situations are where the services of a Community Support Officer can be best utilized. Especially in cases where the perpetrator is the primary provider for the family, an arrest can wreak havoc on a family who was already just victimized. It's important that the victims feel supported, and there are many resources available to victims of domestic violence. For this reason, Chicago Police wants to send out a team with a Community Support Officer when there is a domestic crime where an arrest is likely to occur. 
The Chicago Police Department has designed a system that triggers a dialog box if a 911 operator identifies a reported crime as domestic-related through their electronic system. The dialog box prompts the operator to enter some information about the crime. This information then gets routed to the Department of Community Support, where a staff member determines whether or not this crime warrants the department sending out a Community Support Officer.
### Problem Statement
Unfortunately, in 2017 there were an average of over 100 domestic-related crimes per day in Chicago, and it is not feasible for the Department of Community Support to review all reports. They'd lke to automate some of the decision-making in order to reduce the number of 911-reported crimes that the Department of Community Support staff must review. 
The Chicago Police Department's electronic system allows for them to include check-boxes and drop-down menus in the dialog box that is created when a 911 operator identifies a call as domestic-related. For timeliness and ease of use, the UX designers have put the following limitations on this dialog box:

-There may be no more than 8 check-boxes or drop-down menus.
-A drop-down menu may contain no more than 10 options to choose from, and they should be easy to choose from.

The City of Chicago would like for me to build a model that can predict whether a particular 911 call labeled domestic-related will result in an arrest, because these are the types of situations where a Community Support Officer can best be utilized. They will use this model to design the check-boxes and drop-downs in the 911 operator domestic-related crime dialog box. I will be using the City of Chicago Crime Data to train this model. Due to the use case for this model, it will be important to only select features that *could be included* as a check-box or drop-down menu (as well as location data, which is already collected during all 911 calls). It should alos include only features that the 911-operator is likely to know (e.g. if the domestic incident involves a theft, it's unlikely that the 911 operator will be able to ascertain the value of this theft).
### Goal
The goal of this initial model is to make the best possible prediction of whether or not a domestic crime will end in arrest, in order to reduce the number of 911 calls the Department of Community Support Staff must review while eliminating as few that will end in arrest as possible.
### Methods
In order to achieve this goal, I started by exploring the data, primarily through the interactive dashboards made available by the City of Chicago. I then combed through and cleaned the data. In addition to elimination of some extreme outliers and suspicious data, and combining of some variables, the best result of this project required that I eliminate certain types of information that cannot be *used*. For instance, though some domestic incidents are labeled in this dataset as "Interference with Public Officer," a 911 call for a domestic incident would likely not indicate such a crime - it's more likely that this was what the perpetrator was charged for despite the initial incident being something different. So, this type of information being included in the model would not represent a real, usable solution. 
For variables that would be entered by the 911 operator as a checkbox or dropdown menu, I binarized the variables so I can check what are the most important variables -- I cannot keep them all. I built two simple models, a Lasso Regularized linear regression and XGBoost, so I could easily see what some of the most important binary variables are. I selected the most predictive to include in the final model. These represent variables that would be entered by the 911 operator in the Dialog Box as either a checkbox or dropdown.
### Results and Use
I included as many binary variables as could be included in a logical way that met the above-listed parameters. I ended up with what would result in the following Dialog Box:
SELECT LOCATION (dropdown):

- DAY CARE CENTER
- COMMERCIAL / BUSINESS OFFICE
- GOVERNMENT BUILDING/PROPERTY
- SMALL RETAIL STORE
- SCHOOL, PRIVATE, BUILDING
- NURSING HOME/RETIREMENT HOME
- CTA BUS STOP
- PARK PROPERTY
- HOTEL/MOTEL
- OTHER

ARE ANY OF THE FOLLOWING INVOLVED IN THIS CRIME (check boxes):
- THEFT
- CRIMINAL TRESPASS	
- Aggravated Assault
- Aggravated Battery
- Offenses Against Family

DOES THIS CRIME INVOLVE ANY OF THE FOLLOWING RARER SITUATIONS (dropdown):
- HOMICIDE
- ARSON
- KIDNAPPING
- NONE OF THESE

The ROC AUC score of the model built on pre-2018 data on the 2018 domestic crimes was 0.59. This is not a good score, but it means that the model does improve upon random chance. To quantify what this would mean to the Chicago Police Department, I imagined that they could only look at 20 911 calls per day, and would look at the top 20 predicted by the model to end in an arrest. In 2018 thus far, that would mean they could look at about 4,840. If they looked at a random 4,840 they would, on average, review ones with a 16.5% chance of ending in arrest. Looking at only the top 4,840 predicted by the model, that would improve by around 40%: instead of 16.5%, about 23% of what they reviewed would be crimes that ended in arrest.
### Future Steps
There is plenty of room for improvement in this model. One additional step I would consider looking at is exploring the sub-categories of the crimes. In addition, a better variable representing location might be a KNN-derived location variable (e.g. the odds that the nearest 10 domestic crimes ended in an arrest). I might also look at how the domestic crimes fit into the larger context: what are the other crimes that happen in the area, and what are their odds of arrest? Features derived from this type of information might improve the model performance.
The more important next step, however, is to train a new model based on the data that comes in from the use of the electronic system that this model helped build. While the existing data may discrtize all types of crimes, it may often be the case that multiple things are true (e.g. it is a criminal tresspass AND aggravated assault). It would also be worth testing variables that subject matter experts identify as likely important, even if the current dataset didn't suggest they were important, because their non-inclusion could be a result of limitations of the data.
