<div align="center">
  <img src="https://images.unsplash.com/photo-1563206767-5b18f218e8de?crop=entropy&cs=srgb&fm=jpg&ixid=M3w3NDk1Nzl8MHwxfHNlYXJjaHwyfHxoYWNraW5nfGVufDB8fHx8MTc1NzUxMzQ1N3ww&ixlib=rb-4.1.0&q=85" alt="Hacking Code" width="600"/>
</div>

---

## 🔥 **Matrix Rain Effect**
Create the iconic Matrix digital rain in your terminal (well…browser canvas 😎)!

```javascript
'use strict';

/* ===========================
   🌧️ Matrix Rain Effect
   =========================== */
const createMatrixRain = () => {
  const canvas = document.createElement('canvas');
  const ctx = canvas.getContext('2d');

  const resize = () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  };

  resize();
  window.addEventListener('resize', resize);
  document.body.appendChild(canvas);

  const chars =
    'アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホマミムメモヤユヨラリルレロワヲン0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
  const charArray = chars.split('');

  const drops = [];
  const fontSize = 14;
  const columns = Math.floor(canvas.width / fontSize);

  for (let i = 0; i < columns; i++) {
    drops[i] = 1;
  }

  const draw = () => {
    ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.fillStyle = '#0f0';
    ctx.font = `${fontSize}px monospace`;

    for (let i = 0; i < drops.length; i++) {
      const text = charArray[Math.floor(Math.random() * charArray.length)];
      ctx.fillText(text, i * fontSize, drops[i] * fontSize);

      if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
        drops[i] = 0;
      }
      drops[i]++;
    }
  };

  return setInterval(draw, 35);
};

// 🚀 Launch (uncomment to run in a browser):
// createMatrixRain();

/* ==========================================
   💻 Advanced Async/Await + Retry Backoff
   ========================================== */
class APIHandler {
  constructor(baseURL, maxRetries = 3) {
    this.baseURL = baseURL;
    this.maxRetries = maxRetries;
  }

  async fetchWithRetry(endpoint, options = {}, retryCount = 0) {
    const delay = Math.pow(2, retryCount) * 1000; // Exponential backoff

    try {
      const response = await fetch(`${this.baseURL}${endpoint}`, {
        ...options,
        headers: {
          'Content-Type': 'application/json',
          ...options.headers,
        },
      });

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      return await response.json();
    } catch (error) {
      console.error(`🚨 Attempt ${retryCount + 1} failed:`, error.message);

      if (retryCount < this.maxRetries) {
        console.log(`⏳ Retrying in ${delay}ms...`);
        await new Promise((resolve) => setTimeout(resolve, delay));
        return this.fetchWithRetry(endpoint, options, retryCount + 1);
      }

      throw new Error(`💥 Max retries reached. Final error: ${error.message}`);
    }
  }
}

// 🔧 Usage (example; runs in module/modern browsers):
// (async () => {
//   const api = new APIHandler('https://api.example.com');
//   const data = await api.fetchWithRetry('/users/123');
//   console.log('API data:', data);
// })();

/* ====================================
   🎯 Functional Programming Pipeline
   ==================================== */
const dataTransformPipeline = (...fns) => (value) =>
  fns.reduce((acc, fn) => fn(acc), value);

const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);

// Demo dataset
const users = [
  { id: 1, name: 'Alice', age: 25, active: true, skills: ['JS', 'React'] },
  { id: 2, name: 'Bob', age: 30, active: false, skills: ['Python', 'Django'] },
  { id: 3, name: 'Charlie', age: 35, active: true, skills: ['JS', 'Node'] },
];

const processUsers = dataTransformPipeline(
  // Filter active users
  (arr) => arr.filter((user) => user.active),

  // Add computed properties
  (arr) =>
    arr.map((user) => ({
      ...user,
      isExperienced: user.age > 28,
      skillCount: user.skills.length,
      displayName: `${user.name} (${user.age}y)`,
    })),

  // Sort by experience level then age
  (arr) =>
    arr.sort((a, b) => b.isExperienced - a.isExperienced || b.age - a.age),

  // Group by experience level
  (arr) =>
    arr.reduce((acc, user) => {
      const key = user.isExperienced ? 'senior' : 'junior';
      (acc[key] ||= []).push(user);
      return acc;
    }, {})
);

console.log('🔥 Processed Users:', processUsers(users));

/* ===========================================
   ⚡ ES6+ Destructuring & Param Defaults
   =========================================== */
const complexObject = {
  user: {
    personal: {
      name: 'John',
      age: 30,
      address: {
        street: '123 Main St',
        city: 'New York',
        coordinates: { lat: 40.7128, lng: -74.006 },
      },
    },
    professional: {
      job: 'Developer',
      skills: ['JavaScript', 'React', 'Node.js'],
      experience: {
        years: 5,
        companies: ['TechCorp', 'StartupXYZ'],
      },
    },
  },
  metadata: { created: '2024-01-01', version: '2.1.0' },
};

// 🎯 Destructuring Like a Pro
const {
  user: {
    personal: {
      name: userName,
      address: {
        city,
        coordinates: { lat: latitude, lng: longitude },
      },
    },
    professional: {
      skills: [primarySkill, ...otherSkills],
      experience: { years: experienceYears },
    },
  },
  metadata: { version },
} = complexObject;

console.log(`
🚀 User: ${userName}
📍 Location: ${city} (${latitude}, ${longitude})
💼 Primary Skill: ${primarySkill}
🧰 Other Skills: ${otherSkills.join(', ')}
🎯 Experience: ${experienceYears} years
⚡ Version: ${version}
`);

// 🔥 Function Parameter Destructuring
const createUser = ({
  name,
  email,
  age = 18,
  preferences: { theme = 'dark', notifications = true } = {},
  ...additionalData
}) => ({
  id: Math.random().toString(36).slice(2, 11),
  name,
  email,
  age,
  settings: { theme, notifications },
  metadata: { created: new Date().toISOString(), ...additionalData },
});

// Usage
const newUser = createUser({
  name: 'Alice',
  email: 'alice@example.com',
  preferences: { theme: 'light' },
  department: 'Engineering',
  level: 'Senior',
});
console.log('👤 New User:', newUser);

/* =======================================
   🎨 Real-Time Data Visualization (Canvas)
   ======================================= */
class RealTimeChart {
  constructor(canvasId, options = {}) {
    this.canvas = document.getElementById(canvasId);
    this.ctx = this.canvas.getContext('2d');
    this.data = [];
    this.maxDataPoints = options.maxPoints || 50;
    this.animationId = null;

    this.colors = {
      background: '#1a1a1a',
      grid: '#333',
      line: '#00ff88',
      fill: 'rgba(0, 255, 136, 0.1)',
    };

    this.setupCanvas();
    this.startAnimation();
  }

  setupCanvas() {
    const { canvas } = this;
    const cssWidth = canvas.offsetWidth || 600;
    const cssHeight = canvas.offsetHeight || 300;

    canvas.width = cssWidth * 2;
    canvas.height = cssHeight * 2;
    canvas.style.width = cssWidth + 'px';
    canvas.style.height = cssHeight + 'px';
    this.ctx.scale(2, 2);
  }

  addDataPoint(value) {
    this.data.push({ value, timestamp: Date.now() });
    if (this.data.length > this.maxDataPoints) this.data.shift();
  }

  draw() {
    const { ctx, canvas, data, colors } = this;
    const width = canvas.width / 2;
    const height = canvas.height / 2;

    // Clear
    ctx.fillStyle = colors.background;
    ctx.fillRect(0, 0, width, height);

    if (data.length < 2) return;

    // Grid
    ctx.strokeStyle = colors.grid;
    ctx.lineWidth = 0.5;
    for (let i = 0; i <= 10; i++) {
      const y = (height / 10) * i;
      ctx.beginPath();
      ctx.moveTo(0, y);
      ctx.lineTo(width, y);
      ctx.stroke();
    }

    // Bounds
    const values = data.map((d) => d.value);
    const min = Math.min(...values);
    const max = Math.max(...values);
    const range = max - min || 1;

    // Line
    ctx.strokeStyle = colors.line;
    ctx.lineWidth = 2;
    ctx.beginPath();

    data.forEach((point, index) => {
      const x = (width / (data.length - 1)) * index;
      const y = height - ((point.value - min) / range) * height;
      if (index === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    });

    ctx.stroke();

    // Fill
    ctx.fillStyle = colors.fill;
    ctx.lineTo(width, height);
    ctx.lineTo(0, height);
    ctx.closePath();
    ctx.fill();
  }

  startAnimation() {
    const animate = () => {
      this.draw();
      this.animationId = requestAnimationFrame(animate);
    };
    animate();
  }

  // Simulate real-time data
  simulateData() {
    setInterval(() => {
      const value = Math.sin(Date.now() / 1000) + Math.random() * 0.5;
      this.addDataPoint(value);
    }, 100);
  }
}

// 🚀 Chart demo (requires <canvas id="myChart"> in DOM):
// const chart = new RealTimeChart('myChart');
// chart.simulateData();

/* ======================================
   🎭 Advanced Promise Orchestration
   ====================================== */
class PromiseOrchestrator {
  // Parallel with per-task fallback + timeout
  static async executeWithFallbacks(tasks) {
    const results = await Promise.allSettled(
      tasks.map(async ({ fn, fallback, timeout = 5000 }) => {
        const timeoutPromise = new Promise((_, reject) =>
          setTimeout(() => reject(new Error('Timeout')), timeout)
        );

        try {
          return await Promise.race([fn(), timeoutPromise]);
        } catch (error) {
          console.warn(`Task failed, using fallback:`, error.message);
          return fallback ? await fallback() : null;
        }
      })
    );

    return results.map((result, index) => ({
      index,
      status: result.status,
      value: result.value,
      error: result.reason,
    }));
  }

  // Sequential until first success
  static async executeSequentiallyUntilSuccess(tasks) {
    for (let i = 0; i < tasks.length; i++) {
      try {
        console.log(`🔄 Executing task ${i + 1}/${tasks.length}`);
        const result = await tasks[i]();
        console.log(`✅ Task ${i + 1} succeeded`);
        return { success: true, result, taskIndex: i };
      } catch (error) {
        console.log(`❌ Task ${i + 1} failed:`, error.message);
        if (i === tasks.length - 1) {
          throw new Error(`All ${tasks.length} tasks failed`);
        }
      }
    }
  }

  // Batch processing with concurrency by chunking
  static async processBatches(items, processor, batchSize = 3) {
    const results = [];

    for (let i = 0; i < items.length; i += batchSize) {
      const batch = items.slice(i, i + batchSize);
      console.log(`🔄 Processing batch ${Math.floor(i / batchSize) + 1}`);

      const batchResults = await Promise.all(
        batch.map(async (item, index) => {
          try {
            return await processor(item, i + index);
          } catch (error) {
            return { error: error.message, item };
          }
        })
      );

      results.push(...batchResults);
    }

    return results;
  }
}

// 🎯 Example (requires endpoints):
// (async () => {
//   const tasks = [
//     {
//       fn: () => fetch('/api/primary').then((r) => r.json()),
//       fallback: () => ({ data: 'cached-primary' }),
//       timeout: 3000,
//     },
//     {
//       fn: () => fetch('/api/secondary').then((r) => r.json()),
//       fallback: () => ({ data: 'cached-secondary' }),
//       timeout: 2000,
//     },
//   ];
//   const results = await PromiseOrchestrator.executeWithFallbacks(tasks);
//   console.log('🎯 Results with fallbacks:', results);
// })();

/* ==================================
   🛡️ Security & Validation Helpers
   ================================== */
class SecurityUtils {
  static sanitize(input, options = {}) {
    const { allowHTML = false, maxLength = 1000, stripScripts = true } = options;

    if (typeof input !== 'string') return '';

    let sanitized = input.slice(0, maxLength);

    if (stripScripts) {
      sanitized = sanitized.replace(
        /<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi,
        ''
      );
    }

    if (!allowHTML) {
      sanitized = sanitized
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#x27;');
    }

    return sanitized.trim();
  }

  static createValidator(schema) {
    return (data) => {
      const errors = [];

      for (const [field, rules] of Object.entries(schema)) {
        const value = data[field];

        for (const rule of rules) {
          const result = rule.validate(value);
          if (!result.valid) {
            errors.push({ field, message: result.message, value });
          }
        }
      }

      return { valid: errors.length === 0, errors, data: errors.length ? null : data };
    };
  }
}

const ValidationRules = {
  required: (message = 'Field is required') => ({
    validate: (value) => ({
      valid: value !== undefined && value !== null && value !== '',
      message,
    }),
  }),

  email: (message = 'Invalid email format') => ({
    validate: (value) => ({
      valid: typeof value === 'string' && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
      message,
    }),
  }),

  minLength: (min, message) => ({
    validate: (value) => ({
      valid: typeof value === 'string' && value.length >= min,
      message: message || `Minimum length is ${min}`,
    }),
  }),

  pattern: (regex, message) => ({
    validate: (value) => ({
      valid: typeof value === 'string' && regex.test(value),
      message,
    }),
  }),
};

// 🚀 Validation usage
const userValidator = SecurityUtils.createValidator({
  email: [ValidationRules.required(), ValidationRules.email()],
  password: [
    ValidationRules.required(),
    ValidationRules.minLength(8, 'Password must be at least 8 characters'),
    ValidationRules.pattern(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/,
      'Password must contain lowercase, uppercase, and number'
    ),
  ],
});

const validationResult = userValidator({
  email: 'user@example.com',
  password: 'SecurePass123',
});
console.log('🔐 Validation result:', validationResult);

/* ==================================
   🎪 Performance Monitoring Utility
   ================================== */
class PerformanceMonitor {
  constructor() {
    this.metrics = new Map();
    this.observers = [];
    this.setupObservers();
  }

  measure(name, fn) {
    return async (...args) => {
      const start = performance.now();
      try {
        const result = await fn(...args);
        const duration = performance.now() - start;
        this.recordMetric(name, duration);
        return result;
      } catch (error) {
        const duration = performance.now() - start;
        this.recordMetric(`${name}_error`, duration);
        throw error;
      }
    };
  }

  recordMetric(name, value, unit = 'ms') {
    if (!this.metrics.has(name)) this.metrics.set(name, []);
    this.metrics.get(name).push({ value, timestamp: Date.now(), unit });

    // Keep only last 100 measurements
    const measurements = this.metrics.get(name);
    if (measurements.length > 100) measurements.shift();
  }

  getStats(name) {
    const measurements = this.metrics.get(name);
    if (!measurements || !measurements.length) return null;

    const values = measurements.map((m) => m.value);
    const sum = values.reduce((a, b) => a + b, 0);
    const avg = sum / values.length;
    const min = Math.min(...values);
    const max = Math.max(...values);
    const sorted = [...values].sort((a, b) => a - b);
    const median = sorted[Math.floor(sorted.length / 2)];

    return {
      name,
      count: values.length,
      average: Math.round(avg * 100) / 100,
      median: Math.round(median * 100) / 100,
      min: Math.round(min * 100) / 100,
      max: Math.round(max * 100) / 100,
      unit: measurements[0].unit,
    };
  }

  setupObservers() {
    if (typeof window !== 'undefined' && 'PerformanceObserver' in window) {
      // Long Tasks
      try {
        const longTaskObserver = new PerformanceObserver((list) => {
          for (const entry of list.getEntries()) {
            this.recordMetric('long_task', entry.duration);
            console.warn(`🐌 Long task detected: ${entry.duration.toFixed(2)}ms`);
          }
        });
        longTaskObserver.observe({ entryTypes: ['longtask'] });
      } catch {
        console.log('Long task observation not supported');
      }

      // Navigation Timing
      try {
        const navigationObserver = new PerformanceObserver((list) => {
          for (const entry of list.getEntries()) {
            this.recordMetric('page_load', entry.loadEventEnd - entry.fetchStart);
            this.recordMetric('dom_ready', entry.domContentLoadedEventEnd - entry.fetchStart);
          }
        });
        navigationObserver.observe({ entryTypes: ['navigation'] });
      } catch {
        console.log('Navigation timing observation not supported');
      }
    }
  }

  generateReport() {
    const report = { timestamp: new Date().toISOString(), metrics: {} };
    for (const [name] of this.metrics) {
      report.metrics[name] = this.getStats(name);
    }
    return report;
  }
}

// 🚀 Monitor usage demo
const monitor = new PerformanceMonitor();

const slowFunction = monitor.measure('slow_operation', async () => {
  await new Promise((resolve) => setTimeout(resolve, 100));
  return 'Done!';
});

slowFunction().then((res) => console.log('⏱️ slowFunction:', res));

monitor.recordMetric('api_response_time', 245);
monitor.recordMetric('cache_hit_rate', 0.85, '%');

console.log('📊 Performance Report:', monitor.generateReport());
