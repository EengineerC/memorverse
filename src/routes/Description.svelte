<script lang="ts">
  </script>
    
    <div class="description-panel">
      <h2>Features</h2>
      
      <div class="features">
        <div class="feature">
          <span class="icon">🎯</span>
          <h3>Interactive Practice</h3>
          <p>Test your memory with customizable word hiding</p>
        </div>
        <div class="feature">
          <span class="icon">🎤</span>
          <h3>Voice Recognition</h3>
          <p>Practice verses hands-free using speech</p>
        </div>
        <div class="feature">
          <span class="icon">⚡</span>
          <h3>Customizable</h3>
          <p>Adjust difficulty to match your learning style</p>
        </div>
      </div>
    </div>
    
    <style>
      .description-panel {
        background: white;
        border-radius: 16px;
        padding: 2.5rem;
        margin: 0 auto 2rem;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
        max-width: 1000px;
      }
    
      h2 {
        color: #2c5282;
        font-size: 2rem;
        margin-bottom: 1rem;
        text-align: center;
      }
    
      p {
        color: #4a5568;
        line-height: 1.7;
        font-size: 1.1rem;
        margin-bottom: 2rem;
      }
    
      .features {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 2rem;
        margin-top: 3rem;
      }
    
      .feature {
        text-align: center;
        padding: 1.5rem;
        background: #f8fafc;
        border-radius: 12px;
        transition: transform 0.2s;
        border: 1px solid #e2e8f0;
      }
    
      .feature:hover {
        transform: translateY(-5px);
        box-shadow: 0 4px 12px rgba(44, 82, 130, 0.1);
      }
    
      .icon {
        font-size: 2rem;
        margin-bottom: 1rem;
        display: inline-block;
      }
    
      h3 {
        color: #2c5282;
        font-size: 1.25rem;
        margin-bottom: 0.5rem;
      }
    
      .feature p {
        color: #4a5568;
        font-size: 1rem;
        margin-bottom: 0;
      }
    
      @media (max-width: 768px) {
        .description-panel {
          padding: 1.5rem;
          margin: 1rem;
        }
    
        .features {
          grid-template-columns: 1fr;
          gap: 1rem;
        }
      }
    </style>