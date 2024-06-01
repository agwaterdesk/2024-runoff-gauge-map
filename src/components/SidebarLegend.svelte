<div class="dek legend">
    <div class="legend__item">
      <div class="item__icon">
        <div class="legend__line legend__line--green"></div>
        <div class="legend__circle legend__circle--green"></div>
      </div>
      Flow-normalized water-quality loads
    </div>
  
    <div class="legend__item">
      <div class="item__icon">
        <div class="legend__line legend__line--yellow"></div>
        <div class="legend__circle legend__circle--yellow"></div>
      </div>
      5-year rolling average
    </div>
  </div>
  
  <style lang="scss">

    .dek {
      margin-bottom: 0.5rem;
    }
    .legend {
      display: flex;
      align-items: start;
      gap: 4px;
      flex-direction: column;
      margin-top: 0.5rem;
  
      &__item {
        display: flex;
        align-items: center;
        gap: 8px;
  
        .item__icon {
          display: flex;
          align-items: center;
  
          &--green,
          &--yellow {
            display: inline-block;
          }
        }
  
        .legend__line {
          width: 15px;
          height: 3px;
  
          &--green {
            background: #6da34d;
          }
  
          &--yellow {
            background: #f1b82d;
          }
        }
  
        .legend__circle {
          width: 10px;
          height: 10px;
          border-radius: 50%;
          border-width: 3px;
          border-style: solid;
  
          &--green {
            border-color: #6da34d;
          }
  
          &--yellow {
            border-color: #f1b82d;
          }
        }
      }
    }
  </style>
  