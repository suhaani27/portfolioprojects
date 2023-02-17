select * from portfolio..coviddeaths$
order by 3,4

--select * from portfolio..covidvaccinations$
--order by 3,4

-- select data that we are going to be using 

select location,date,total_cases,new_cases,total_deaths,population
from portfolio..coviddeaths$
order by 1,2

--looking at the total cases vs total deaths
-- shows the likelihood of dying if you contract the virus in India 
select location,date,total_cases,total_deaths, (total_deaths/total_cases)*100 as deathpercentage
from portfolio..coviddeaths$
where location like 'india'
order by 1,2


--looking at the total cases vs population
-- shows what population has gotten covid

select location,date,population, total_cases, (total_cases/population)*100 as percentagepopulationinfected
from portfolio..coviddeaths$
where location like 'india'
order by 1,2


--looking at  countries with highest infection rate compared to population
select location,population, max(total_cases) as highestinfectioncount, max((total_cases/population))*100 as percentagepopulationinfected
from portfolio..coviddeaths$
group by location,population
order by percentagepopulationinfected desc

--showing the countries with the highest death count per population
select location, max(cast(total_deaths as int)) as totaldeathcount 
from portfolio..coviddeaths$
where continent is not null
group by location
order by totaldeathcount desc

-- breaking data down by continent
select continent, max(cast(total_deaths as int)) as totaldeathcount 
from portfolio..coviddeaths$
where continent is not null
group by continent
order by totaldeathcount desc

-- by location and sorting continent 
select location, max(cast(total_deaths as int)) as totaldeathcount 
from portfolio..coviddeaths$
where continent is null
group by location
order by totaldeathcount desc

--global numbers
select date, sum(new_cases), sum(cast(new_deaths as int))
from portfolio..coviddeaths$
where continent is not null
group by date
order by 1,2

--new deaths and new cases %
select date, sum(new_cases) as total_cases, sum(cast(new_deaths as int)) as total_deaths, (sum(cast(new_deaths as int))/sum(new_cases))*100 as deathpercentage
from portfolio..coviddeaths$
where continent is not null
group by date
order by 1,2

--looking at total cases vs total deaths as percentage as a whole
select sum(new_cases) as total_cases, sum(cast(new_deaths as int)) as total_deaths, (sum(cast(new_deaths as int))/sum(new_cases))*100 as deathpercentage
from portfolio..coviddeaths$
where continent is not null
order by 1,2

--joining our two table - coviddeaths and covidvaccinations
select *
from portfolio..coviddeaths$ dea
join portfolio..covidvaccinations$ vac
	on dea.location = vac.location
	and dea.date = vac.date

	--looking at total population vs vaccinations
	select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
from portfolio..coviddeaths$ dea
join portfolio..covidvaccinations$ vac
	on dea.location = vac.location
	and dea.date = vac.date
	order by 1,2,3

	--parting by location
		select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, sum(cast(vac.new_vaccinations as int)) over (partition by dea.location)
from portfolio..coviddeaths$ dea
join portfolio..covidvaccinations$ vac
	on dea.location = vac.location
	and dea.date = vac.date
	where dea.continent is not null
	order by 1,2,3

	--partition by location and date 
		select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, 
		sum(cast(vac.new_vaccinations as int)) over (partition by dea.location order by dea.location, dea.date) as rollingpeoplevaccinations, (rollingpeoplevaccinations/population)*100
from portfolio..coviddeaths$ dea
join portfolio..covidvaccinations$ vac
	on dea.location = vac.location
	and dea.date = vac.date
	where dea.continent is not null
	order by 1,2,3 

	 --using CTE to add column within the command here a new column is being created 
	 with popvsvac (continent, population,date,location, new_vaccinations, rollingpeoplevaccinations)
	 as
(
	 select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, sum(cast(vac.new_vaccinations as int)) over (partition by dea.location order by dea.location, dea.date) as rollingpeoplevaccinations --, (rollingpeoplevaccinations/population)*100
from portfolio..coviddeaths$ dea
join portfolio..covidvaccinations$ vac
	on dea.location = vac.location
	and dea.date = vac.date
	where dea.continent is not null
)
select * 
from popvsvac






