<template>
  <div></div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  name: "Modal",
  data() {
    return {
      isInfoModalOpen: false,
      isTooltipPinned: false,

      selectedTab: "card",
    selectedArtworks: [],
    searchQuery: "",
    sortBy: "default",
    preorderPrint: false,
    balance: 105,
    participationCost: 36,
    nextWorkCost: 18,
    isInfoModalOpen: false,
    isTooltipPinned: false,
    artworks: [
      { id: 1, price: 14850, image: "/images/artworks/artwork1.jpg" },
      { id: 2, price: 14850, image: "/images/artworks/artwork2.jpg" },
      { id: 3, price: 14850, image: "/images/artworks/artwork3.jpg" },
      { id: 4, price: 14850, image: "/images/artworks/artwork4.jpg" },
      { id: 5, price: 14850, image: "/images/artworks/artwork1.jpg" },
      { id: 6, price: 14850, image: "/images/artworks/artwork2.jpg" },
      { id: 7, price: 14850, image: "/images/artworks/artwork3.jpg" },
      { id: 8, price: 14850, image: "/images/artworks/artwork4.jpg" },
      { id: 9, price: 14850, image: "/images/artworks/artwork1.jpg" },
      { id: 10, price: 14850, image: "/images/artworks/artwork2.jpg" },
      { id: 11, price: 14850, image: "/images/artworks/artwork3.jpg" },
      { id: 12, price: 14850, image: "/images/artworks/artwork4.jpg" },
    ],
    };
  },
  methods: {
    setupMasonryLayout() {
      const items = document.querySelectorAll(".artwork-item");

      items.forEach((item) => {
        const img = item.querySelector(".artwork-image");

        img.addEventListener("load", () => {
          const height = img.offsetHeight;
          const spans = Math.ceil((height + 16) / 10);
          item.style.gridRowEnd = `span ${spans}`;
        });

        // Если изображение уже загружено
        if (img.complete) {
          const height = img.offsetHeight;
          const spans = Math.ceil((height + 16) / 10);
          item.style.gridRowEnd = `span ${spans}`;
        }
      });
    },

    showModal() {
      Swal.fire({
        html: this.getModalContent(),
        showConfirmButton: false,
        showCloseButton: true,
        customClass: {
          popup: "art-modal",
          closeButton: "art-modal__close",
        },
        width: "1384px",
        didOpen: () => {
          this.initModalEvents();
        },
      });
    },

    getModalContent() {
      return `
    <div class="art-modal-content">
      <!-- Header section -->
      <div class="art-modal__header">
        <div class="art-modal__search-section">
          <div class="search-input-wrapper">
            <input 
              type="text" 
              placeholder="Поиск" 
              class="search-input"
              id="searchInput"
              value="${this.searchQuery}"
            >
            <button class="search-button" id="searchBtn">
              <svg width="18" height="18" viewBox="0 0 16 16">
                <path fill="#604093" d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
              </svg>
            </button>
          </div>
          
          <div class="sort-dropdown">
            <img src="/images/frame.png" alt="Sort icon" class="sort-left-icon">
            <select class="sort-select" id="sortSelect">
              <option value="default">По умолчанию</option>
              <option value="price">По цене</option>
              <option value="name">По названию</option>
            </select>
            <svg class="sort-arrow" width="16" height="17" viewBox="0 0 16 17" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M2.7193 6.46658L7.06596 10.8132C7.5793 11.3266 8.4193 11.3266 8.93263 10.8132L13.2793 6.46658" stroke="#3F3F3F" stroke-width="1.5" stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round"/>
</svg>

          </div>
        </div>

        <div class="art-modal__balance">
          <span class="balance-label">Ваш баланс:</span>
          <span class="balance-amount">${this.balance} ₽</span>
        </div>
      </div>

      <!-- Payment and Top-up section -->
      <div class="art-modal__payment-section">

        <div class="art-modal__payment">
          <div class="payment-label">Оплата:</div>
          <div class="payment-tabs">
            <button class="payment-tab ${
              this.selectedTab === "card" ? "active" : ""
            }" data-tab="card">
              По карте
            </button>
            <button class="payment-tab ${
              this.selectedTab === "points" ? "active" : ""
            }" data-tab="points">
              Баллами
            </button>
          </div>
        </div>
        
        <button class="balance-btn" id="topUpBtn">Пополнить</button>
      </div>

      <!-- Info section -->
      <div class="art-modal__info">
        <div class="info-item">
          <span class="info-label">Стоимость участия (для первых двух работ)</span>
          <span class="info-dash"> - </span>
          <span class="info-value">${this.participationCost}</span>
        </div>
        <div class="info-item">
          <span class="info-label">Каждая последующая</span>
          <span class="info-dash"> - </span>
          <span class="info-value">${this.nextWorkCost}</span>
        </div>
      </div>

      <!-- Counters section -->
      <div class="art-modal__counters">
        <div class="counter-item">
          <span class="counter-label">Выбранные работы</span>
          <span class="counter-dash"> - </span>
          <span class="counter-value" id="selectedCount">${
            this.selectedArtworks.length
          }</span>
        </div>
        <div class="counter-item">
          <span class="counter-label">Вы должны внести</span>
          <span class="counter-dash"> - </span>
          <span class="counter-value" id="totalAmount">${this.calculateTotal()}</span>
        </div>
      </div>

      <!-- Preorder checkbox -->
      <div class="art-modal__preorder">
        <!-- Новый код без label -->
<div class="preorder-checkbox">
  <div class="checkbox-wrapper">
    <input type="checkbox" id="preorderCheck" ${this.preorderPrint ? "checked" : ""}>
    <span class="checkmark" id="checkmarkSpan"></span>
  </div>
  <span class="checkbox-label">Оформить предзаказ на печатную версию</span>
  <img 
    src="${this.isInfoModalOpen ? "/images/vopros-active.png" : "/images/vopros.png"}" 
    alt="Info" 
    class="info-icon" 
    id="infoIcon"
    width="20" 
    height="20"
  >
</div>
      </div>

      <!-- Gallery section -->
      <div class="art-modal__gallery" id="artGallery">
        ${this.renderGallery()}
      </div>

      <!-- Footer buttons -->
      <div class="art-modal__footer">
        <button class="btn btn-primary" id="confirmBtn">Подтвердить</button>
        <button class="btn btn-secondary" id="backBtn">Назад</button>
      </div>
    </div>
  `;
    },

    showInfoModal(pinned = false) {
      this.isInfoModalOpen = true;
      this.isTooltipPinned = pinned;

      // Обновляем иконку
      const infoIcon = document.getElementById("infoIcon");
      if (infoIcon) {
        infoIcon.src = "/images/vopros-active.png";
      }

      // Создаем tooltip если его еще нет
      if (!document.querySelector(".custom-tooltip")) {
        this.createTooltip();
      }
    },

    hideTooltip() {
      const tooltip = document.querySelector(".custom-tooltip");
      if (tooltip) {
        // Анимация исчезновения
        tooltip.style.opacity = "0";
        tooltip.style.transform = "translateY(-10px) scale(0.95)";

        // Удаляем элемент после завершения анимации
        setTimeout(() => {
          if (tooltip.parentNode) {
            tooltip.remove();
          }
        }, 200);
      }

      this.isInfoModalOpen = false;
      this.isTooltipPinned = false;

      // Возвращаем обычную иконку
      const infoIcon = document.getElementById("infoIcon");
      if (infoIcon) {
        infoIcon.src = "/images/vopros.png";
      }

      if (this.tooltipClickHandler) {
        document.removeEventListener("click", this.tooltipClickHandler);
      }
    },

    createTooltip() {
      // Удаляем существующий tooltip если есть
      const existingTooltip = document.querySelector(".custom-tooltip");
      if (existingTooltip) {
        existingTooltip.remove();
      }

      const tooltip = document.createElement("div");
      tooltip.className = "custom-tooltip";
      tooltip.innerHTML = `
    <div class="tooltip-arrow"></div>
    <div class="tooltip-content">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
    </div>
  `;

      // Добавляем tooltip в DOM с начальной невидимостью
      tooltip.style.opacity = "0";
      tooltip.style.transform = "translateY(-10px) scale(0.95)";
      document.body.appendChild(tooltip);

      // Позиционируем tooltip относительно иконки
      const infoIcon = document.getElementById("infoIcon");
      const iconRect = infoIcon.getBoundingClientRect();

      // Получаем реальные размеры tooltip
      const tooltipRect = tooltip.getBoundingClientRect();

      // Позиционируем tooltip снизу от иконки
      tooltip.style.left =
        iconRect.left + iconRect.width / 2 - tooltipRect.width / 2 + "px";
      tooltip.style.top = iconRect.bottom + 10 + "px";

      // Позиционируем стрелочку точно по центру иконки
      const arrow = tooltip.querySelector(".tooltip-arrow");
      arrow.style.left = tooltipRect.width / 2 - 8 + "px";

      // Запускаем анимацию появления
      requestAnimationFrame(() => {
        tooltip.style.opacity = "1";
        tooltip.style.transform = "translateY(0) scale(1)";
      });

      // Обработчики для самого tooltip
      tooltip.addEventListener("mouseenter", () => {
        this.isInfoModalOpen = true;
      });

      tooltip.addEventListener("mouseleave", () => {
        if (!this.isTooltipPinned) {
          this.hideTooltip();
        }
      });

      // Создаем обработчик с правильным контекстом (только для закрепленных tooltip)
      this.tooltipClickHandler = (e) => {
        const tooltip = document.querySelector(".custom-tooltip");
        const infoIcon = document.getElementById("infoIcon");

        if (
          this.isTooltipPinned &&
          tooltip &&
          !tooltip.contains(e.target) &&
          e.target !== infoIcon
        ) {
          this.hideTooltip();
        }
      };

      document.addEventListener("click", this.tooltipClickHandler);
    },

    closeTooltip(e) {
      const tooltip = document.querySelector(".custom-tooltip");
      const infoIcon = document.getElementById("infoIcon");

      if (tooltip && !tooltip.contains(e.target) && e.target !== infoIcon) {
        tooltip.remove();
        this.isInfoModalOpen = false;

        // Возвращаем обычную иконку
        if (infoIcon) {
          infoIcon.src = "/images/vopros.png";
        }

        document.removeEventListener("click", this.closeTooltip);
      }
    },

   renderGallery() {
  return this.artworks
    .map(artwork => `
      <div class="artwork-item" data-id="${artwork.id}">
        <img src="${artwork.image}" alt="Artwork ${artwork.id}" class="artwork-image">
        <div class="artwork-price">#${artwork.price}</div>
        <div class="artwork-overlay"></div>
      </div>
    `)
    .join("");
},

    initModalEvents() {
      // Обработчики для табов оплаты
      document.querySelectorAll(".payment-tab").forEach((tab) => {
        tab.addEventListener("click", (e) => {
          this.selectedTab = e.target.dataset.tab;
          this.updatePaymentTabs();
        });
      });

      // Обработчик для чекбокса
      document.getElementById("preorderCheck").addEventListener("change", (e) => {
        this.preorderPrint = e.target.checked;
      });

      // Обработчик для галереи
      document.querySelectorAll(".artwork-item").forEach((item) => {
        item.addEventListener("click", (e) => {
          const artworkId = parseInt(e.currentTarget.dataset.id);
          this.toggleArtworkSelection(artworkId);
        });
      });

      // Обработчик поиска
      document.getElementById("searchInput").addEventListener("input", (e) => {
        this.searchQuery = e.target.value;
        // Реализуйте фильтрацию
      });

      // Обработчики кнопок
      document.getElementById("confirmBtn").addEventListener("click", () => {
        this.confirmSelection();
      });

      document.getElementById("backBtn").addEventListener("click", () => {
        Swal.close();
      });

      document.getElementById("topUpBtn").addEventListener("click", () => {
        // Логика пополнения баланса
        console.log("Пополнить баланс");
      });

      // Обработчики для информационной иконки
      const infoIcon = document.getElementById("infoIcon");

      // Наведение мыши - показать tooltip с задержкой
      let hoverTimeout;
      infoIcon.addEventListener("mouseenter", (e) => {
        clearTimeout(hoverTimeout);
        if (!this.isInfoModalOpen) {
          hoverTimeout = setTimeout(() => {
            this.showInfoModal(false); // false = не закреплен
          }, 100);
        }
      });

      // Уход мыши - скрыть tooltip только если не закреплен
      infoIcon.addEventListener("mouseleave", (e) => {
        clearTimeout(hoverTimeout);
        if (this.isInfoModalOpen && !this.isTooltipPinned) {
          setTimeout(() => {
            if (!this.isTooltipPinned && this.isInfoModalOpen) {
              this.hideTooltip();
            }
          }, 100);
        }
      });

      // Клик - закрепить/открепить tooltip
      infoIcon.addEventListener("click", (e) => {
        e.preventDefault();
        e.stopPropagation();

        if (this.isInfoModalOpen && this.isTooltipPinned) {
          // Если tooltip закреплен - убираем его
          this.hideTooltip();
        } else {
          // Показываем и закрепляем tooltip
          this.showInfoModal(true); // true = закреплен
        }
      });

      // Обработчик для чекбокса (только для самого чекбокса и checkmark)
      document.getElementById("preorderCheck").addEventListener("change", (e) => {
        this.preorderPrint = e.target.checked;
      });

      // Обработчик для checkmark
      document.getElementById("checkmarkSpan").addEventListener("click", (e) => {
        const checkbox = document.getElementById("preorderCheck");
        checkbox.checked = !checkbox.checked;
        this.preorderPrint = checkbox.checked;
      });

      this.setupMasonryLayout();
    },

    updatePaymentTabs() {
      document.querySelectorAll(".payment-tab").forEach((tab) => {
        tab.classList.toggle("active", tab.dataset.tab === this.selectedTab);
      });
    },

    toggleArtworkSelection(artworkId) {
      const index = this.selectedArtworks.indexOf(artworkId);
      if (index > -1) {
        this.selectedArtworks.splice(index, 1);
      } else {
        this.selectedArtworks.push(artworkId);
      }

      // Обновляем UI
      this.updateCounters();
      this.updateArtworkSelection(artworkId);
    },

    updateArtworkSelection(artworkId) {
      const artworkElement = document.querySelector(`[data-id="${artworkId}"]`);
      if (artworkElement) {
        artworkElement.classList.toggle(
          "selected",
          this.selectedArtworks.includes(artworkId)
        );
      }
    },

    updateCounters() {
      const selectedCountElement = document.getElementById("selectedCount");
      const totalAmountElement = document.getElementById("totalAmount");

      if (selectedCountElement)
        selectedCountElement.textContent = this.selectedArtworks.length;
      if (totalAmountElement) totalAmountElement.textContent = this.calculateTotal();
    },

    calculateTotal() {
      const count = this.selectedArtworks.length;
      if (count === 0) return 0;
      if (count <= 2) return this.participationCost;
      return this.participationCost + (count - 2) * this.nextWorkCost;
    },

    confirmSelection() {
      // Логика подтверждения выбора
      console.log("Selected artworks:", this.selectedArtworks);
      console.log("Total amount:", this.calculateTotal());
      console.log("Payment method:", this.selectedTab);
      console.log("Preorder print:", this.preorderPrint);
      Swal.close();
    },
  },
};
</script>

<style src="../assets/modal.css"></style>
