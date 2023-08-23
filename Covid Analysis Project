-- ANALYSIS ON CASES AND DEATH

-- Select all columns
SELECT *
FROM CovidDeaths
WHERE continent IS NOT NULL
ORDER BY location, date;

-- Select specific columns for analysis
SELECT
    location,
    date,
    total_cases,
    new_cases,
    total_deaths,
    population
FROM CovidDeaths
WHERE continent IS NOT NULL
ORDER BY location, date;

-- Calculate Death Percentage
SELECT
    location,
    date,
    total_cases,
    total_deaths,
    (total_deaths / total_cases) * 100 AS DeathPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL
ORDER BY location, date;

-- Calculate Infected Percentage
SELECT
    location,
    date,
    total_cases,
    population,
    (total_cases / population) * 100 AS InfectedPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL
ORDER BY location, date;

-- Countries with highest infection rate compared to population
SELECT
    location,
    population,
    MAX(total_cases) AS HighestInfectionCount,
    MAX((total_cases / population)) * 100 AS PercentageInfected
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population
ORDER BY PercentageInfected DESC;

-- Countries with highest death count
SELECT
    location,
    MAX(CAST(Total_Deaths AS INT)) AS TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY TotalDeathCount DESC;

-- Get Total Death recorded by each continent
SELECT 
    continent, 
    MAX(CAST(Total_Deaths AS INT)) AS TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY TotalDeathCount DESC;

-- GLOBAL DAILY NUMBERS
SELECT
    date,
    SUM(new_cases) AS total_cases,
    SUM(CAST(new_deaths AS INT)) AS total_deaths,
    CASE
        WHEN SUM(new_cases) = 0 THEN 0 -- Handling divide by zero
        ELSE (SUM(CAST(new_deaths AS INT)) / SUM(new_cases)) * 100
    END AS death_percentage
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY date
ORDER BY date, total_cases;

-- TOTAL NUMBERS TILL REPORT WAS DONE
SELECT
    SUM(new_cases) AS total_cases,
    SUM(CAST(new_deaths AS INT)) AS total_deaths,
    CASE
        WHEN SUM(new_cases) = 0 THEN 0 -- Handling divide by zero
        ELSE (SUM(CAST(new_deaths AS INT)) / SUM(new_cases)) * 100
    END AS death_percentage
FROM CovidDeaths
WHERE continent IS NOT NULL;

-- ANALYSIS ON VACCINATION

-- Joining the 2 tables together
SELECT *
FROM CovidDeaths dea
JOIN CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date;

-- Looking at the Total Population vs Vaccinations worldwide
SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(CONVERT(BIGINT, vac.new_vaccinations)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS total_vaccinations
FROM
    CovidDeaths dea
JOIN
    CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date
WHERE
    dea.continent IS NOT NULL
ORDER BY
    dea.location, dea.date;

-- Looking at the Total Population vs Vaccinations in Nigeria
SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(CONVERT(BIGINT, vac.new_vaccinations)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS total_vaccinations
FROM
    CovidDeaths dea
JOIN
    CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date
WHERE
    dea.location LIKE '%Nigeria%' AND
    dea.continent IS NOT NULL
ORDER BY
    dea.location, dea.date;

-- USING CTE TO STORE VALUE
-- % of people vaccinated in each country
WITH PopvsVac (Continent, Location, Date, Population, new_vaccinations, total_vaccinations)
AS (
    SELECT
        dea.continent,
        dea.location,
        dea.date,
        dea.population,
        vac.new_vaccinations,
        SUM(CONVERT(BIGINT, vac.new_vaccinations)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS total_vaccinations
    FROM
        CovidDeaths dea
    JOIN
        CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date
    WHERE
        dea.continent IS NOT NULL
)
SELECT *, (total_vaccinations / population) * 100 AS RollinPercentage
FROM PopvsVac;

-- % of people vaccinated in NIGERIA
WITH PopvsVac (Continent, Location, Date, Population, new_vaccinations, total_vaccinations)
AS (
    SELECT
        dea.continent,
        dea.location,
        dea.date,
        dea.population,
        vac.new_vaccinations,
        SUM(CONVERT(BIGINT, vac.new_vaccinations)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS total_vaccinations
    FROM
        CovidDeaths dea
    JOIN
        CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date
    WHERE
        dea.location LIKE '%Nigeria%' AND
        dea.continent IS NOT NULL
)
SELECT *, (total_vaccinations / population) * 100 AS RollinPercentage
FROM PopvsVac;

-- TEMP TABLE
DROP TABLE IF EXISTS #PercentPopulationVaccinated;
CREATE TABLE #PercentPopulationVaccinated (
    Continent NVARCHAR(255),
    Location NVARCHAR(255),
    Date DATETIME,
    Population NUMERIC,
    new_vaccinations NUMERIC,
    total_vaccinations NUMERIC
);

INSERT INTO #PercentPopulationVaccinated
SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(CONVERT(BIGINT, vac.new_vaccinations)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS total_vaccinations
FROM
    CovidDeaths dea
JOIN
    CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date
WHERE
    dea.continent IS NOT NULL
ORDER BY
    dea.location, dea.date;

SELECT *, (total_vaccinations / population) * 100 AS RollinPercentage
FROM #PercentPopulationVaccinated;

-- Create view to store data for visualization
CREATE VIEW PercentPopulationVaccinated AS
SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(CONVERT(BIGINT, vac.new_vaccinations)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS total_vaccinations
FROM
    CovidDeaths dea
JOIN
    CovidVaccinations vac ON dea.location = vac.location AND dea.date = vac.date
WHERE
    dea.continent IS NOT NULL;

-- Create view for Infected Percentage
CREATE VIEW InfectedPercentage AS
SELECT
    location,
    date,
    total_cases,
    population,
    (total_cases / population) * 100 AS InfectedPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL

-- View for Country with highest infected rate
CREATE VIEW MostInfectedCountrybyPercentage AS
SELECT
    location,
    population,
    MAX(total_cases) AS HighestInfectionCount,
    MAX((total_cases / population)) * 100 AS PercentageInfected
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population

-- Countries with highest death count
CREATE VIEW CountriesDeathCount AS
SELECT
    location,
    MAX(CAST(Total_Deaths AS INT)) AS TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location

-- Get Total Death recorded by each continent
CREATE VIEW ContinentDeathCount AS
SELECT 
    continent, 
    MAX(CAST(Total_Deaths AS INT)) AS TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY continent
