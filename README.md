# Similar but Unique: An NLP Model to Predict a Subreddit Post as Nike or Adidas
Prepared by Jessie Owens


## Executive Summary

- [Problem Statement](#Problem_Statement)
- [Data Dictionary](#Data_Dictionary)
- [Methodology](#Methodology)
- [Conclusions](#Conclusions)
- [Sources](#Sources)

---

## Problem Statement

Using the Ames housing sales data from 2006-2010, I am analyzing which factor(s) predict housing sale price. Additionally, I am looking at how sale prices differ across neighborhoods within Ames and which factors in the data (or not in the data) might contribute to these differences in prices.

---

## Data Dictionary 

The following are the variables included in the train and test datasets. This data was collected from the Ames Assessor's Office on residential properties sold in Ames, IA from 2006 to 2010 (source provided at end of notebook). 


|Index|Feature|Type|Description|Key|
|---|---|---|---|---|
|1|Id|Int|Observation number||
|2|PID|Int|Parcel identification number  - can be used with city web site for parcel review||
|3|MS SubClass|Int|Identifies the type of dwelling involved in the sale.|020=1-STORY 1946 & NEWER ALL STYLES; 030=1-STORY 1945 & OLDER; 040=1-STORY W/FINISHED ATTIC ALL AGES; 045=1-1/2 STORY - UNFINISHED ALL AGES; 050=1-1/2 STORY FINISHED ALL AGES; 060=2-STORY 1946 & NEWER; 070=2-STORY 1945 & OLDER; 075=2-1/2 STORY ALL AGES; 080=SPLIT OR MULTI-LEVEL; 085=SPLIT FOYER; 090=DUPLEX - ALL STYLES AND AGES; 120=1-STORY PUD (Planned Unit Development) - 1946 & NEWER; 150=1-1/2 STORY PUD - ALL AGES; 160=2-STORY PUD - 1946 & NEWER; 180=PUD - MULTILEVEL - INCL SPLIT LEV/FOYER; 190=2 FAMILY CONVERSION - ALL STYLES AND AGES|
|4|MS Zoning|Object|Identifies the general zoning classification of the sale.|A=Agriculture; C=Commercial; FV=Floating Village Residential; I=Industrial; RH=Residential High Density; RL=Residential Low Density; RP=Residential Low Density Park; RM=Residential Medium Density|
|5|Lot Frontage|Float|Linear feet of street connected to property||
|6|Lot Area|Int|Lot size in square feet||
|7|Street|Object|Type of road access to property|Grvl=Gravel; Pave=Paved|
|8|Alley|Object|Type of alley access to property|Grvl=Gravel; Pave=Paved; NA=No alley access|
|9|Lot Shape|Object|General shape of property|Reg=Regular; IR1=Slightly irregular; IR2=Moderately Irregular; IR3=Irregular|
|10|Land Contour|Object|Flatness of the property|Lvl=Near Flat/Level	; Bnk=Banked - Quick and significant rise from street grade to buildingl HLS=Hillside - Significant slope from side to side; Low=Depression|
|11|Utilities|Object|Type of utilities available|AllPub=All public Utilities (E,G,W,& S)	; NoSewr=Electricity, Gas, and Water (Septic Tank); NoSeWa=Electricity and Gas Only; ELO=Electricity only|
|12|Lot Config|Object|Lot configuration|Inside=Inside lot; Corner=Corner lot; CulDSac=Cul-de-sac; FR2=Frontage on 2 sides of property; FR3=Frontage on 3 sides of property|
|13|Land Slope|Object|Slope of property|Gtl=Gentle slope; Mod=Moderate Slope; Sev=Severe Slope|
|14|Neighborhood|Object|Physical locations within Ames city limits (map available)|Blmngtn=Bloomington Heights; Blueste=Bluestem; BrDale=Briardale; BrkSide=Brookside; ClearCr=Clear Creek; CollgCr=College Creek; Crawfor=Crawford; Edwards= Edwards; Gilbert=Gilbert; Greens=Greens; GrnHill=Green Hills; IDOTRR=Iowa DOT and Rail Road; Landmrk=Landmark; MeadowV=Meadow Village; Mitchel=Mitchell; Names=North Ames; NoRidge=Northridge; NPkVill=Northpark Villa; NridgHt=Northridge Heights; NWAmes=Northwest Ames; OldTown=Old Town; SWISU=South & West of Iowa State University; Sawyer=Sawyer; SawyerW=Sawyer West; Somerst=Somerset; StoneBr=Stone Brook; Timber=Timberland; Veenker=Veenker|
|15|Condition 1|Object|Proximity to various conditions|Artery=Adjacent to arterial street; Feedr=Adjacent to feeder street; Norm=Normal; RRNn=Within 200' of North-South Railroad; RRAn=Adjacent to North-South Railroad; PosN=Near positive off-site feature--park, greenbelt, etc.; PosA=Adjacent to postive off-site feature; RRNe=Within 200' of East-West Railroad; RRAe=Adjacent to East-West Railroad|
|16|Condition 2|Object|Proximity to various conditions (if more than one is present)|Artery=Adjacent to arterial street; Feedr=Adjacent to feeder street; Norm=Normal; RRNn=Within 200' of North-South Railroad; RRAn=Adjacent to North-South Railroad; PosN=Near positive off-site feature--park, greenbelt, etc.; PosA=Adjacent to postive off-site feature; RRNe=Within 200' of East-West Railroad; RRAe=Adjacent to East-West Railroad|
|17|Bldg Type|Object|Type of dwelling|1Fam=Single-family Detached; 2FmCon=Two-family Conversion; originally built as one-family dwelling; Duplx=Duplex; TwnhsE=Townhouse End Unit; TwnhsI=Townhouse Inside Unit|
|18|House Style|Object|Style of dwelling|1Story=One story; 1.5Fin=One and one-half story: 2nd level finished; 1.5Unf=One and one-half story: 2nd level unfinished; 2Story=Two story; 2.5Fin=Two and one-half story: 2nd level finished; 2.5Unf=Two and one-half story: 2nd level unfinished; SFoyer=Split Foyer; SLvl=Split Level|
|19|Overall Qual|Int|Rates the overall material and finish of the house|10=Very Excellent; 9=Excellent; 8=Very Good; 7=Good; 6=Above Average	; 5=Average; 4=Below Average	; 3=Fair; 2=Poor; 1=Very Poor|
|20|Overall Cond|Int|Rates the overall condition of the house|10=Very Excellent; 9=Excellent; 8=Very Good; 7=Good; 6=Above Average	; 5=Average; 4=Below Average	; 3=Fair; 2=Poor; 1=Very Poor|
|21|Year Built|Int|Original construction date||
|22|Year Remod/Add|Int|Remodel date (same as construction date if no remodeling or additions)||
|23|Roof Style|Object|Type of roof|Flat=Flat; Gable=Gable; Gambrel=Gabrel (Barn); Hip=Hip; Mansard=Mansard; Shed=Shed|
|24|Roof Matl|Object|Roof material|ClyTile=Clay or Tile; CompShg=Standard (Composite) Shingle; Membran=Membrane; Metal=Metal; Roll=Roll; Tar&Grv=Gravel & Tar; WdShake=Wood Shakes; WdShngl=Wood Shingles|
|25|Exterior 1st|Object|Exterior covering on house|AsbShng=Asbestos Shingles; AsphShn=Asphalt Shingles; BrkComm=Brick Common; BrkFace=Brick Face; CBlock=Cinder Block; CemntBd=Cement Board; HdBoard=Hard Board; ImStucc=Imitation Stucco; MetalSd	Metal Siding; Other=Other; Plywood=Plywood; PreCast=PreCast; Stone=Stone; Stucco=Stucco; VinylSd=Vinyl Siding; Wd Sdng=Wood Siding; WdShing=Wood Shingles|
|26|Exterior 2nd|Object|Exterior covering on house (if more than one material)|AsbShng=Asbestos Shingles; AsphShn=Asphalt Shingles; BrkComm=Brick Common; BrkFace=Brick Face; CBlock=Cinder Block; CemntBd=Cement Board; HdBoard=Hard Board; ImStucc=Imitation Stucco; MetalSd	Metal Siding; Other=Other; Plywood=Plywood; PreCast=PreCast; Stone=Stone; Stucco=Stucco; VinylSd=Vinyl Siding; Wd Sdng=Wood Siding; WdShing=Wood Shingles|
|27|Mas Vnr Type|Object|Masonry veneer type|BrkCmn=Brick Common; BrkFace=Brick Face; CBlock=Cinder Block; None=None; Stone=Stone|
|28|Mas Vnr Area|Float|Masonry veneer area in square feet||
|29|Exter Qual|Int|Evaluates the quality of the material on the exterior|5=Ex=Excellent; 4=Gd=Good; 3=TA=Average/Typical; 2=Fa=Fair; 1=Po=Poor|
|30|Exter Cond|Int|Evaluates the present condition of the material on the exterior|5=Ex=Excellent; 4=Gd=Good; 3=TA=Average/Typical; 2=Fa=Fair; 1=Po=Poor|
|31|Foundation|Object|Type of Foundation|BrkTil=Brick & Tile; CBlock=Cinder Block; PConc=Poured Concrete; Slab=Slab; Stone=Stone; Wood=Wood|
|32|Bsmt Qual|Int|Evalutes the height of the basement|5=Ex=Excellent (100+ inches); 4=Gd=Good (90-99 inches); 3=TA=Typical (80-89 inches); 2=Fa=Fair (70-79 inches); 1=Po=Poor (<70 inches); 0=NA=No Basement|
|33|Bsmt Cond|Int|Evaluates the general condition of the basement|5=Ex=Excellent; 4=Gd=Good; 3=TA=Typical - slight dampness allowed; 2=Fa=Fair - dampness or some cracking or settling; 1=Po=Poor - Severe cracking, settling, or wetness; 0=NA=No Basement|
|34|Bsmt Exposure|Object|Refers to walkout or garden level walls|Gd=Good Exposure; Av=Average Exposure (split levels or foyers typically score average or above); Mn=Mimimum Exposure; No=No Exposure; NA=No Basement|
|35|BsmtFin Type 1|Object|Rating of basement finished area|GLQ=Good Living Quarters; ALQ=Average Living Quarters; BLQ=Below Average Living Quarters; Rec=Average Rec Room; LwQ=Low Quality; Unf=Unfinshed; NA=No Basement|action|
|36|BsmtFin SF 1|Float|Type 1 finished square feet||
|37|BsmtFinType 2|Object|Rating of basement finished area (if multiple types)|GLQ=Good Living Quarters; ALQ=Average Living Quarters; BLQ=Below Average Living Quarters; Rec=Average Rec Room; LwQ=Low Quality; Unf=Unfinshed; NA=No Basement|
|38|BsmtFin SF 2|Float|Type 2 finished square feet||
|39|Bsmt Unf SF|Float|Unfinished square feet of basement area||
|40|Total Bsmt SF|FLoat|Total square feet of basement area||
|41|Heating|Object|Type of heating|Floor=Floor Furnace; GasA=Gas forced warm air furnace; GasW=Gas hot water or steam heat; Grav=Gravity furnace; OthW=Hot water or steam heat other than gas; Wall=Wall furnace|
|42|HeatingQC|Int|Heating quality and condition|5=Ex=Excellent; 4=Gd=Good; 3=TA=Average/Typical; 2=Fa=Fair; 1=Po=Poor|
|43|Central Air|Object|Central air conditioning|N=No; Y=Yes|
|44|Electrical|Object|Electrical system|SBrkr=Standard Circuit Breakers & Romex; FuseA=Fuse Box over 60 AMP and all Romex wiring (Average); FuseF=60 AMP Fuse Box and mostly Romex wiring (Fair); FuseP=60 AMP Fuse Box and mostly knob & tube wiring (poor); Mix=Mixed|
|45|1st Flr SF|Int|First Floor square feet||
|46|2nd Flr SF|Int|Second floor square feet||
|47|Low Qual Fin SF|Int|Low quality finished square feet (all floors)||
|48|Gr Liv Area|Int|Above grade (ground) living area square feet||
|49|Bsmt Full Bath|Float|Basement full bathrooms||
|50|Bsmt Half Bath|Float|Basement half bathrooms||
|51|Full Bath|Int|Full bathrooms above grade||
|52|Half Bath|Int|Half baths above grade||action|
|53|Bedroom AbvGr|Int|Bedrooms above grade (does NOT include basement bedrooms)||
|54|Kitchen AbvGr|Int|Kitchens above grade||
|55|KitchenQual|Int|Kitchen quality|5=Ex=Excellent; 4=Gd=Good; 3=TA=Average/Typical; 2=Fa=Fair; 1=Po=Poor|
|56|TotRmsAbvGrd|Int|Total rooms above grade (does not include bathrooms)||
|57|Functional|Object|Home functionality (Assume typical unless deductions are warranted)|Typ=Typical Functionality; Min1=Minor Deductions 1; Min2=Minor Deductions 2; Mod=Moderate Deductions; Maj1=Major Deductions 1; Maj2=Major Deductions 2; Sev=Severely Damaged; Sal=Salvage only|
|58|Fireplaces|Int|Number of fireplaces||
|59|FireplaceQu|Object|Fireplace quality|Ex=Excellent - Exceptional Masonry Fireplace; Gd=Good - Masonry Fireplace in main level; TA=Average - Prefabricated Fireplace in main living area or Masonry Fireplace in basement; Fa=Fair - Prefabricated Fireplace in basement; Po=Poor - Ben Franklin Stove; NA=No Fireplace|	
|60|Garage Type|Object|Garage location|2Types=More than one type of garage; Attchd=Attached to home; Basment=Basement Garage; BuiltIn=Built-In (Garage part of house - typically has room above garage); CarPort=Car Port; Detchd=Detached from home; NA=No Garage|
|61|Garage Yr Blt|Float|Year garage was built||
|62|Garage Finish|Object|Interior finish of the garage|Fin=Finished; RFn=Rough Finished; Unf=Unfinished; NA=No Garage|
|63|Garage Cars|Float|Size of garage in car capacity||
|64|Garage Area|Float|Size of garage in square feet||
|65|Garage Qual|Object|Garage quality|Ex=Excellent; Gd=Good; TA=Average/Typical; Fa=Fair; Po=Poor; NA=No Garage|
|66|Garage Cond|Object|Garage condition|Ex=Excellent; Gd=Good; TA=Average/Typical; Fa=Fair; Po=Poor; NA=No Garage|
|67|Paved Drive|Object|Paved driveway|Y=Paved; P=Partial Pavement; N=Dirt/Gravel|		
|68|Wood Deck SF|Int|Wood deck area in square feet|
|69|Open Porch SF|Int|Open porch area in square feet|
|70|Enclosed Porch|Int|Enclosed porch area in square feet|
|71|3-Ssn Porch|Int|Three season porch area in square feet|
|72|Screen Porch|Int|Screen porch area in square feet|
|73|Pool Area|Int|Pool area in square feet|
|74|Pool QC|Object|Pool quality|Ex=Excellent; Gd=Good; TA=Average/Typical; Fa=Fair; NA=No Pool||action|
|75|Fence|Object|Fence quality|GdPrv=Good Privacy; MnPrv=Minimum Privacy; GdWo=Good Wood; MnWw=Minimum Wood/Wire; NA=No Fence|
|76|Misc Feature|Object|Miscellaneous feature not covered in other categories|Elev=Elevator; Gar2=2nd Garage (if not described in garage section); Othr=Other; Shed=Shed (over 100 SF); TenC=Tennis Court; NA=None|	
|77|Misc Val|Int|$Value of miscellaneous feature||
|78|Mo Sold|Int|Month Sold (MM)||
|79|Yr Sold|Int|Year Sold (YYYY)||
|80|Sale Type|Object|Type of sale|WD=Warranty Deed - Conventional; CWD=Warranty Deed - Cash; VWD=Warranty Deed - VA Loan; New=Home just constructed and sold; COD=Court Officer Deed/Estate; Con=Contract 15% Down payment regular terms; ConLw=Contract Low Down payment and low interest' ConLI=Contract Low Interest; ConLD=Contract Low Down; Oth=Other|
|81|SalePrice|Int|Sale price $$ (Target Variable||

---

## Methodology

1. Import & clean data
2. Exploratory data analysis
3. Feature Engineering
4. External Research
5. Modeling
6. Conclusions

---

## Conclusions

Setting a home sale price is a complex process. There is no one feature to consider that could determine the price on it's own. Square footage, whether it be of the entire home or specifically above ground, is a good predictor of sale prices, but it should not be the sole factor. As other features increment - bathrooms or halfbathrooms, outdoor space in the form of a deck, garage space, finished basements, number of fireplaces, neighborhood - a sale price can start to rise. When someone is buying a home, they never say "This house has 1,700 square feet, I'm sold". It's normally something more like "This house has 1,700 square feet, 2.5 bathrooms, 3 bedrooms, a fireplace, a 2 car garage, and is located near one of the best elementary schools in the city, I'll buy it."

I primarily used linear regression in my modeling phase to predict prices on the test data. I also tested out using k nearest neighbors and gridsearch to optimize the hyperparameters, however I got a realy bad score so went back to Linear Regression. I would have liked to try some other models, but at this point I feel most comfortable with using the Linear Regression Model and will come back to maybe add some models to this later on.

I also would maybe try to map neighborhoods to relevant elementary schools and their ranking in the city or state with the education department. I think this might be powerful in predicting prices.

---

## Sources

- http://jse.amstat.org/v19n3/decock/DataDocumentation.txt

- https://www.tandfonline.com/doi/full/10.1080/10691898.2008.11889569

---