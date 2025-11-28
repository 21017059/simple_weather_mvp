// API KEY
const API_KEY = "c930f6cdaa430f04e573bc843634c9b8";
const UNSPLASH_ACCESS_KEY = "ePwYJpCLjr2DthQcaTKekXetE38KPbOGEEoVVrdX9Hs";

window.addEventListener("DOMContentLoaded", () => {

  // 이름 입력 처리
  const nameInput = document.getElementById("userNameInput");
  const setNameBtn = document.getElementById("setNameBtn");
  const nameContainer = document.getElementById("nameContainer");

  setNameBtn.addEventListener("click", () => {
    const userName = nameInput.value.trim();
    if (userName) {
      document.getElementById("title").textContent = `🌤 ${userName}님 만의 날씨`;
      nameContainer.style.display = "none"; // 입력 후 숨김
    }
  });

  // 도시 검색 처리
  const searchBtn = document.getElementById("searchBtn");

  // 검색 처리 함수
  function handleSearch() {
    const city = document.getElementById("cityInput").value.trim();
    if (!city) {
      alert("도시명을 입력해주세요!");
      return;
    }
    fetchWeather(city);
    fetchForecast(city);
  }

  // PC click + 모바일 touchend 이벤트 모두 등록
  searchBtn.addEventListener("click", handleSearch);
  searchBtn.addEventListener("touchend", handleSearch);

});

// --- Unsplash 도시 이미지 불러오기 ---
async function fetchCityImage(city) {
  const url = `https://api.unsplash.com/search/photos?query=${city}&client_id=${UNSPLASH_ACCESS_KEY}&orientation=landscape`;

  try {
    const res = await fetch(url);
    const data = await res.json();

    if (!data.results || data.results.length === 0) {
      console.log("도시에 대한 이미지가 없습니다.");
      return;
    }

    const imageUrl = data.results[0].urls.regular;
    document.body.style.backgroundImage = `url(${imageUrl})`;
    document.body.style.backgroundSize = "cover";
    document.body.style.backgroundPosition = "center";
    document.body.style.backgroundRepeat = "no-repeat";

  } catch (error) {
    console.error("이미지 가져오기 오류:", error);
  }
}

// --- 현재 날씨 ---
async function fetchWeather(city) {
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric&lang=kr`;

  try {
    const res = await fetch(url);
    const data = await res.json();

    if (data.cod == "404") {
      alert("도시를 찾을 수 없습니다. 올바른 도시명을 입력해주세요.");
      return;
    }

    displayWeather(data);
    updateWeatherimage(data.main.temp);
    fetchCityImage(city);

  } catch (error) {
    console.error(error);
    alert("날씨 데이터를 불러오는 중 오류가 발생했습니다.");
  }
}

// --- 3일 예보 ---
async function fetchForecast(city) {
  const url = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${API_KEY}&units=metric&lang=kr`;

  try {
    const res = await fetch(url);
    const data = await res.json();

    if (data.cod == "404") return;

    const selected = data.list.filter((item, index) => index % 8 === 0).slice(0, 3);
    const simplified = selected.map(item => ({
      day: new Date(item.dt * 1000).toLocaleDateString("ko-KR", { weekday: "short" }),
      temp: item.main.temp,
      icon: item.weather[0].icon,
    }));

    displayForecast(simplified);

  } catch (error) {
    console.error(error);
    alert("예보 데이터를 불러오는 중 오류가 발생했습니다.");
  }
}

// --- 현재 날씨 화면 업데이트 ---
function displayWeather(data) {
  document.getElementById("temp").textContent = data.main.temp + "°C";
  document.getElementById("desc").textContent = data.weather[0].description;
  document.getElementById("humidity").textContent = data.main.humidity;
  document.getElementById("wind").textContent = data.wind.speed;
  document.getElementById("weatherIcon").src =
    `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
}

// --- 온도별 캐릭터 이미지 ---
function updateWeatherimage(temp) {
  const img = document.getElementById("weather-image");
  const comment = document.getElementById("weather");

  if (temp >= 30) {
    img.src = "images/hot.png";
    comment.textContent = "날씨가 더우니 조심하세요!";
  } else if (temp >= 20) {
    img.src = "images/warm.png";
    comment.textContent = "따뜻한 날씨네요!";
  } else if (temp >= 10) {
    img.src = "images/cool.png";
    comment.textContent = "선선한 날씨에요!";
  } else {
    img.src = "images/cold.png";
    comment.textContent = "날씨가 춥네요! 옷 따뜻하게 입으세요!";
  }
}

// --- 3일 예보 카드 ---
function displayForecast(list) {
  const container = document.getElementById("forecastContainer");
  container.innerHTML = "";

  list.forEach(item => {
    const card = document.createElement("div");
    card.className = "card";
    card.innerHTML = `
      <p>${item.day}</p>
      <img src="https://openweathermap.org/img/wn/${item.icon}.png">
      <p>${item.temp}°C</p>
    `;
    container.appendChild(card);
  });
}
