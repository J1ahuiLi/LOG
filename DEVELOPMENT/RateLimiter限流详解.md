# RateLimiter 限流详解

`RateLimiter` 是一种用于控制资源访问速率的工具，通常用于限制系统在单位时间内处理的请求数量，以防止系统过载或资源耗尽。`RateLimiter` 广泛应用于高并发系统、API 限流、流量控制等场景。

在 Java 中，`RateLimiter` 是 **Google Guava 库** 提供的一个实现。下面我们将详细讲解 `RateLimiter` 的原理、使用方法和应用场景。

## RateLimiter 的核心概念

### 什么是 RateLimiter？

`RateLimiter` 是一种限流器，用于控制单位时间内允许的操作数量。它基于**令牌桶算法**（Token Bucket Algorithm）或**漏桶算法**（Leaky Bucket Algorithm）实现。

- **令牌桶算法**：
  - 系统以固定速率向桶中添加令牌。
  - 每个请求需要消耗一个令牌。
  - 如果桶中没有足够的令牌，请求将被限流。
- **漏桶算法**：
  - 请求以固定速率从桶中漏出。
  - 如果桶已满，新的请求将被限流。

Guava 的 `RateLimiter` 是基于令牌桶算法实现的。

### 核心特性

- **平滑突发限制**（Smooth Bursty）：
  - 允许短时间内突发请求，但长期平均速率不超过设定值。
- **平滑预热限制**（Smooth Warming Up）：
  - 在系统启动时，逐步增加速率，避免冷启动时的流量冲击。
- **非阻塞式限流**：
  - 支持尝试获取令牌，如果令牌不足，可以选择等待或直接拒绝。

## Guava RateLimiter 的使用

### 添加依赖

如果使用 Maven，需要在 `pom.xml` 中添加 Guava 依赖：

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>31.0.1-jre</version>
</dependency>
```

运行 HTML

### 创建 RateLimiter

通过 `RateLimiter.create()` 方法创建一个限流器，指定每秒允许的请求数（QPS）。

```java
import com.google.common.util.concurrent.RateLimiter;

public class RateLimiterExample {
    public static void main(String[] args) {
        // 创建一个每秒允许 2 个请求的限流器
        RateLimiter rateLimiter = RateLimiter.create(2.0);

        for (int i = 1; i <= 10; i++) {
            // 尝试获取令牌
            if (rateLimiter.tryAcquire()) {
                System.out.println("处理请求: " + i);
            } else {
                System.out.println("请求被限流: " + i);
            }
        }
    }
}
```

### 核心方法

**1. `RateLimiter.create(double permitsPerSecond)`**

- 创建一个平滑突发限制的限流器。
- 参数 `permitsPerSecond` 表示每秒允许的请求数（QPS）。

**2. `RateLimiter.create(double permitsPerSecond, long warmupPeriod, TimeUnit unit)`**

- 创建一个平滑预热限制的限流器。
- 参数 `warmupPeriod` 表示预热时间。

**3. `rateLimiter.acquire()`**

- 阻塞当前线程，直到获取到令牌。
- 返回等待的时间（秒）。

**4. `rateLimiter.tryAcquire()`**

- 尝试获取令牌，如果令牌不足，立即返回 `false`。
- 不会阻塞线程。

**5. `rateLimiter.tryAcquire(int permits, long timeout, TimeUnit unit)`**

- 尝试在指定时间内获取指定数量的令牌。
- 如果超时仍未获取到令牌，返回 `false`。

### 示例代码

#### 示例 1：平滑突发限制

```java
import com.google.common.util.concurrent.RateLimiter;

public class SmoothBurstyExample {
    public static void main(String[] args) {
        // 每秒允许 2 个请求
        RateLimiter rateLimiter = RateLimiter.create(2.0);

        for (int i = 1; i <= 10; i++) {
            // 获取令牌，如果令牌不足则阻塞
            double waitTime = rateLimiter.acquire();
            System.out.println("处理请求: " + i + ", 等待时间: " + waitTime + " 秒");
        }
    }
}
```

#### 示例 2：平滑预热限制

```java
import com.google.common.util.concurrent.RateLimiter;
import java.util.concurrent.TimeUnit;

public class SmoothWarmingUpExample {
    public static void main(String[] args) {
        // 每秒允许 2 个请求，预热时间为 3 秒
        RateLimiter rateLimiter = RateLimiter.create(2.0, 3, TimeUnit.SECONDS);

        for (int i = 1; i <= 10; i++) {
            double waitTime = rateLimiter.acquire();
            System.out.println("处理请求: " + i + ", 等待时间: " + waitTime + " 秒");
        }
    }
}
```

#### 示例 3：非阻塞式限流

```java
import com.google.common.util.concurrent.RateLimiter;

public class NonBlockingExample {
    public static void main(String[] args) {
        // 每秒允许 2 个请求
        RateLimiter rateLimiter = RateLimiter.create(2.0);

        for (int i = 1; i <= 10; i++) {
            // 尝试获取令牌，如果令牌不足则直接返回
            if (rateLimiter.tryAcquire()) {
                System.out.println("处理请求: " + i);
            } else {
                System.out.println("请求被限流: " + i);
            }
        }
    }
}
```

## RateLimiter 的应用场景

### API 限流

- 防止 API 被恶意请求或突发流量打垮。
- 例如，限制每个用户每秒只能调用某个 API 10 次。

### 资源保护

- 限制对数据库、文件系统等共享资源的访问速率，避免资源耗尽。

### 流量控制

- 在微服务架构中，限制服务之间的调用速率，防止雪崩效应。

### 任务调度

- 控制任务处理的速率，确保系统负载均衡。

## RateLimiter 的底层原理

### 令牌桶算法

- 系统以固定速率向桶中添加令牌。
- 每个请求需要消耗一个令牌。
- 如果桶中没有足够的令牌，请求将被限流。

### 实现细节

- Guava 的 `RateLimiter` 使用了一种称为**“存储桶”**的机制，动态调整令牌的生成速率。
- 支持突发流量（短时间内消耗多个令牌），但长期平均速率不超过设定值。

## RateLimiter 的优缺点

### 优点

- 简单易用，集成方便。
- 支持突发流量和预热机制。
- 高性能，适合高并发场景。

### 缺点

- 单机限流，无法直接支持分布式限流。
- 需要根据实际场景调整参数，否则可能导致限流效果不理想。

## 分布式限流

如果需要在分布式系统中实现限流，可以使用以下方案：

- **Redis + Lua 脚本**：通过 Redis 的原子操作实现分布式限流。
- **Sentinel**：阿里巴巴开源的分布式限流组件。
- **Nginx 限流**：通过 Nginx 的限流模块实现网关层限流。

## 总结

- `RateLimiter` 是 Google Guava 提供的一个限流工具，基于令牌桶算法实现。
- 支持平滑突发限制和平滑预热限制，适用于 API 限流、资源保护等场景。
- 在单机场景下性能优异，但在分布式场景中需要结合其他工具实现限流。

通过合理使用 `RateLimiter`，可以有效保护系统免受突发流量的冲击，提升系统的稳定性和可靠性。

## 限制每个用户每秒只能调用某个 API 10 次。用 RateLimiter 实现

我们可以为每个用户创建一个独立的 `RateLimiter` 实例，并将其存储在缓存（如 `ConcurrentHashMap`）中，以便快速查找和更新。 以下是完整的实现代码：

```java
import com.google.common.util.concurrent.RateLimiter;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.TimeUnit;

public class UserRateLimiter {

    // 使用 ConcurrentHashMap 存储每个用户的 RateLimiter
    private static final ConcurrentHashMap<String, RateLimiter> userRateLimiters = new ConcurrentHashMap<>();

    // 每秒允许的请求数
    private static final double PERMITS_PER_SECOND = 10.0;

    /**
     * 检查用户是否允许调用 API
     *
     * @param userId 用户 ID
     * @return true 表示允许调用，false 表示限流
     */
    public static boolean allowRequest(String userId) {
        // 获取用户的 RateLimiter，如果不存在则创建一个新的
        RateLimiter rateLimiter = userRateLimiters.computeIfAbsent(userId, k -> RateLimiter.create(PERMITS_PER_SECOND));

        // 尝试获取令牌
        return rateLimiter.tryAcquire();
    }

    /**
     * 清理长时间未使用的 RateLimiter，避免内存泄漏
     */
    public static void cleanUpInactiveUsers() {
        userRateLimiters.entrySet().removeIf(entry -> {
            // 如果 RateLimiter 长时间未被使用，则移除
            return entry.getValue().getRate() == 0; // 这里可以根据实际需求设计更复杂的清理逻辑
        });
    }

    public static void main(String[] args) throws InterruptedException {
        // 模拟用户调用 API
        String userId = "user123";

        for (int i = 1; i <= 15; i++) {
            boolean allowed = allowRequest(userId);
            if (allowed) {
                System.out.println("请求 " + i + ": 允许调用");
            } else {
                System.out.println("请求 " + i + ": 被限流");
            }
            // 模拟请求间隔
            TimeUnit.MILLISECONDS.sleep(50);
        }

        // 清理不活跃的用户
        cleanUpInactiveUsers();
    }
}
```

### 优化建议

1. **分布式限流**：
   - 如果系统是分布式的，单机的 `RateLimiter` 无法满足需求。
   - 可以使用 Redis 实现分布式限流，例如通过 Redis 的 `INCR` 和 `EXPIRE` 命令。
2. **清理策略**：
   - 需要定期清理不活跃用户的 `RateLimiter`，避免内存泄漏。
   - 可以使用定时任务（如 `ScheduledExecutorService`）定期清理。
3. **动态调整速率**：
   - 如果需要根据用户等级或业务需求动态调整速率，可以扩展 `RateLimiter` 的创建逻辑。
4. **限流提示**：
   - 当请求被限流时，可以返回提示信息（如 HTTP 429 Too Many Requests）。