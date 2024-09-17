# Nashville Housing Data (Data Cleaning Project)

## OverView

In this project, I focused on importing and cleaning Nashville housing data within a SQL database. The process began with the extraction of raw data, which was then imported into My SQL Server. Following the import, I performed a thorough data cleaning process to address inconsistencies, remove duplicates, and ensure data accuracy. This involved standardizing data formats, correcting errors, and validating entries to prepare the dataset for further analysis and reporting. The result is a refined dataset ready for in depth exploration and insightful decision making.

## Tools Used
SQL
Excel

## Data Cleaning
 --Data Cleaning

  Select * 
  From [Portfolio_Project].[dbo].[Nashville_Housing]

  --Changing Sale Date

  Select SaleDateConverted, CONVERT(Date,SaleDate)
  From [Portfolio_Project].[dbo].[Nashville_Housing]

  Update Nashville_Housing
  SET SaleDate = CONVERT(Date,SaleDate)

ALTER TABLE Nashville_Housing
Add SaleDateConverted Date;

Update Nashville_Housing
SET SaleDateConverted = CONVERT(Date,SaleDate)

--Populate Property Adress Data

Select *
  From [Portfolio_Project].[dbo].[Nashville_Housing]
  --Where PropertyAddress is null 

  order by ParcelID



 Select a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)
 From [Portfolio_Project].[dbo].[Nashville_Housing] a
 JOIN [Portfolio_Project].[dbo].[Nashville_Housing] b
 on a.ParcelID = b.ParcelID
 AND a.[UniqueID ] <> b. [UniqueID ]
 Where a.PropertyAddress is null

 Update a
 SET PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
 From [Portfolio_Project].[dbo].[Nashville_Housing] a
 JOIN [Portfolio_Project].[dbo].[Nashville_Housing] b
 on a.ParcelID = b.ParcelID
 AND a.[UniqueID ] <> b. [UniqueID ]
 Where a.PropertyAddress is null


 -- Breaking Out Address Into Individual Columns (Address, City, State)



 Select PropertyAddress
  From [Portfolio_Project].[dbo].[Nashville_Housing]
  --Where PropertyAddress is null 
--order by ParcelID

SELECT
SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress)-1) as Address
, SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress)) as Address
 From [Portfolio_Project].[dbo].[Nashville_Housing]

 ALTER TABLE Nashville_Housing
 Add PropertySplitAddress Nvarchar(255);

 Update Nashville_Housing
 SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1)

 ALTER TABLE Nashville_Housing
 Add PropertySplitCity Nvarchar(255);

 Update Nashville_Housing
 SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress))


 Select * 
 From [Portfolio_Project].[dbo].[Nashville_Housing]


 Select
 PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)
 ,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)
 ,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)
 From Portfolio_Project.dbo.Nashville_Housing

 ALTER TABLE Nashville_Housing
 Add OwnerSplitAddress Nvarchar(255);

 Update Nashville_Housing
 SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)

 ALTER TABLE Nashville_Housing
 Add OwnerSplitCity Nvarchar(255);

 Update Nashville_Housing
 SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)

 ALTER TABLE Nashville_Housing
 Add OwnerSplitState Nvarchar(255);

 Update Nashville_Housing
 SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)


 Select * 
 From [Portfolio_Project].[dbo].[Nashville_Housing]

 --Change Y and N and No in"Sold as Vacant" field

 Select Distinct(SoldAsVacant), count(SoldAsVacant)
 From [Portfolio_Project].[dbo].[Nashville_Housing]
 Group by SoldAsVacant
 Order by 2

 Select SoldAsVacant
 , CASE When SoldAsVacant = 'Y' THEN 'YES'
        When SoldAsVacant = 'N' THEN 'NO'
		ELSE SoldAsVacant
		END
 From [Portfolio_Project].[dbo].[Nashville_Housing]

 Update [Nashville_Housing] 
 SET SoldAsVacant = CASE When SoldAsVacant = 'Y' THEN 'YES'
        When SoldAsVacant = 'N' THEN 'NO'
		ELSE SoldAsVacant
		END


-- Deleting Unused Columns

Select *
From [Portfolio_Project].[dbo].[Nashville_Housing]


ALTER TABLE [Portfolio_Project].[dbo].[Nashville_Housing]
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress

ALTER TABLE [Portfolio_Project].[dbo].[Nashville_Housing]
DROP COLUMN SaleDate
