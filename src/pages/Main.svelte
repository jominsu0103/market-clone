<script>
  import { onMount } from "svelte";
  import { getDatabase, ref, onValue } from "firebase/database";
  import Nav from "../components/Nav.svelte";
  

  let hour = new Date().getHours();
  let min = new Date().getMinutes();

  $: items = [];

  const calcTime = (timestamp) => {
    const curTime = new Date().getTime() - 9 * 60 * 60 * 1000;
    const time = new Date(curTime - timestamp);
    const hour = time.getHours();
    const minute = time.getMinutes();
    const second = time.getSeconds();

    if (hour > 0) return `${hour}시간 전`;
    else if (minute > 0) return `${minute}분 전`;
    else if (second > 0) return `${second}초 전`;
    else return "방금 전";
  };

  const db = getDatabase();
  const itemsRef = ref(db, "items/");
  onMount(() => {
    onValue(itemsRef, (snapshot) => {
      const data = snapshot.val();
      items = Object.values(data).reverse();
    });
  });
</script>

<header>
  <div class="info-bar">
    <div class="info-bar_time">{hour}:{min}</div>
    <div class="info-bar_icons">
      <img src="assets/char-bar.svg" alt="chart-bar" />
      <img src="assets/wifi.svg" alt="wifi" />
      <img src="assets/battery.svg" alt="battery" />
    </div>
  </div>
  <div class="menu-bar">
    <div class="menu-bar_location">
      <div>역삼1동</div>
      <div class="menu-bar_location-icon">
        <img src="assets/arrow-down.svg" alt="" />
      </div>
    </div>
    <div class="menu-bar_icons">
      <img src="assets/search.svg" alt="" />
      <img src="assets/menu.svg" alt="" />
      <img src="assets/alert.svg" alt="" />
    </div>
  </div>
</header>
<main>
  <!-- <div class="item-list">
    <div class="item-list_img">
      <img src="assets/image.svg" alt="" />
    </div>
    <div class="item-list_info">
      <div class="item-list_info-title">게이밍 pc 팝니다</div>
      <div class="item-list_info-meta">역삼동 19초 전</div>
      <div class="item-list_info-price">100만원</div>
    </div>
  </div>
  <div class="item-list">
    <div class="item-list_img">
      <img src="assets/image.svg" alt="" />
    </div>
    <div class="item-list_info">
      <div class="item-list_info-title">게이밍 pc 팝니다</div>
      <div class="item-list_info-meta">역삼동 19초 전</div>
      <div class="item-list_info-price">100만원</div>
    </div>
  </div>
  <div class="item-list">
    <div class="item-list_img">
      <img src="assets/image.svg" alt="" />
    </div>
    <div class="item-list_info">
      <div class="item-list_info-title">게이밍 pc 팝니다</div>
      <div class="item-list_info-meta">역삼동 19초 전</div>
      <div class="item-list_info-price">100만원</div>
    </div>
  </div>
  <div class="item-list">
    <div class="item-list_img">
      <img src="assets/image.svg" alt="" />
    </div>
    <div class="item-list_info">
      <div class="item-list_info-title">게이밍 pc 팝니다</div>
      <div class="item-list_info-meta">역삼동 19초 전</div>
      <div class="item-list_info-price">100만원</div>
      <div></div>
    </div>
  </div>
  <div class="item-list">
    <div class="item-list_img">
      <img src="assets/image.svg" alt="" />
    </div>
    <div class="item-list_info">
      <div class="item-list_info-title">게이밍 pc 팝니다</div>
      <div class="item-list_info-meta">역삼동 19초 전</div>
      <div class="item-list_info-price">100만원</div>
    </div>
  </div>
  <div class="item-list">
    <div class="item-list_img">
      <img src="assets/image.svg" alt="" />
    </div>
    <div class="item-list_info">
      <div class="item-list_info-title">게이밍 pc 팝니다</div>
      <div class="item-list_info-meta">역삼동 19초 전</div>
      <div class="item-list_info-price">100만원</div>
    </div>
  </div> -->
  <a href="#/write" class="write-btn">+ 글쓰기</a>

  {#each items as item}
    <div class="item-list">
      <div class="item-list_img">
        <img src={item.imgUrl} alt={item.title} />
      </div>
      <div class="item-list_info">
        <div class="item-list_info-title">{item.title}</div>
        <div class="item-list_info-price">
          {item.price}
        </div>
        <div class="item-list_info-meta">
          {item.place}
          {calcTime(item.insertAt)}
        </div>
        <div>{item.description}</div>
      </div>
    </div>
  {/each}
</main>
<Nav location="home" />
<div class="media-info-msg">화면 사이즈를 줄여주세요</div>

<style>
  .info-bar_time {
    color: blue;
  }
</style>
