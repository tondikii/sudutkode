<script setup lang="ts">
import {
  Card,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import {
  Carousel,
  CarouselContent,
  CarouselItem,
  CarouselNext,
  CarouselPrevious,
} from "@/components/ui/carousel";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { Bell, Wallet, BarChart3 } from "lucide-vue-next";

type ProductStatus = "available" | "coming-soon";

interface Screenshot {
  src: string;
  caption: string;
}

interface Product {
  id: string;
  name: string;
  tagline: string;
  description: string;
  platform: string;
  screenshots: Screenshot[];
  downloadUrl: string;
  status: ProductStatus;
  icon?: string;
  features?: Array<{
    icon: any;
    label: string;
  }>;
}

const products: Product[] = [
  {
    id: "uangku",
    name: "UangKu",
    tagline: "Personal Finance Tracker",
    description:
      "A personal finance tracking app with visual reports, transaction categories, and automatic tracker. Manage your finances easily and efficiently.",
    platform: "Android",
    icon: "/projects/uangku/uangku-icon.png",
    screenshots: [
      {
        src: "/projects/uangku/uangku-auth.jpg",
        caption: "Sign in securely with Google OAuth 2.0",
      },
      {
        src: "/projects/uangku/uangku-intro.jpg",
        caption: "Automatic transaction recording from banking & e-wallet notifications",
      },
      {
        src: "/projects/uangku/uangku-access-request.jpg",
        caption: "Reads only banking & e-wallet notifications — no personal messages",
      },
      {
        src: "/projects/uangku/uangku-initial-wallets.jpg",
        caption: "Set up your wallets: cash, m-banking, and e-wallets in one place",
      },
      {
        src: "/projects/uangku/uangku-transactions.jpg",
        caption: "Daily transaction history with income, expense, and balance summary",
      },
      {
        src: "/projects/uangku/uangku-wallets.jpg",
        caption: "Monitor all wallet balances at a glance",
      },
      {
        src: "/projects/uangku/uangku-report.jpg",
        caption: "Monthly spending reports with category breakdown",
      },
      {
        src: "/projects/uangku/uangku-create-transaction.jpg",
        caption: "Manually add transactions with category, wallet, and date",
      },
      {
        src: "/projects/uangku/uangku-settings.jpg",
        caption: "Supports BCA, Livin, BRImo, Jago, SeaBank, GoPay, OVO, ShopeePay, DANA",
      },
    ],
    downloadUrl: "https://expo.dev/artifacts/eas/i4XJixtKxwVsTyW5djGnkK.apk",
    status: "available",
    features: [
      {
        icon: Bell,
        label: "Auto-record from notifications",
      },
      {
        icon: Wallet,
        label: "Multi-wallet support",
      },
      {
        icon: BarChart3,
        label: "Monthly reports",
      },
    ],
  },
];

const getBadgeVariant = (status: ProductStatus) => {
  return status === "coming-soon" ? "secondary" : "default";
};

const getButtonText = (status: ProductStatus) => {
  return status === "coming-soon" ? "Coming Soon" : "Download";
};

const isButtonDisabled = (status: ProductStatus) => {
  return status === "coming-soon";
};
</script>

<template>
  <section
    id="products"
    class="container mx-auto px-4 md:px-6 py-20 md:py-32 lg:py-40"
  >
    <div class="max-w-3xl mx-auto text-center mb-12">
      <h2 class="text-3xl md:text-4xl lg:text-5xl font-semibold leading-tight">
        Our Products
      </h2>
    </div>

    <div class="space-y-12 max-w-7xl mx-auto">
      <div
        v-for="product in products"
        :key="product.id"
      >
        <!-- Card without overflow-hidden so arrows are not clipped -->
        <Card class="border-border">
          <div class="flex flex-col lg:flex-row gap-8 p-6 md:p-8">

            <!-- Left: Product Info (~40% on desktop) -->
            <div class="flex flex-col lg:max-w-[38%]">
              <CardHeader class="p-0 mb-6">
                <div class="mb-3">
                  <div class="flex items-center gap-3 mb-2">
                    <img
                      v-if="product.icon"
                      :src="product.icon"
                      :alt="`${product.name} icon`"
                      class="w-10 h-10 rounded-lg"
                    />
                    <CardTitle class="text-2xl md:text-3xl">
                      {{ product.name }}
                    </CardTitle>
                  </div>
                  <CardDescription class="text-base">
                    {{ product.tagline }}
                  </CardDescription>
                </div>
                <Badge
                  :variant="getBadgeVariant(product.status)"
                  class="w-fit"
                >
                  {{ product.platform }}
                </Badge>
              </CardHeader>

              <p class="text-sm md:text-base text-muted-foreground leading-relaxed mb-6">
                {{ product.description }}
              </p>

              <!-- Features -->
              <div
                v-if="product.features && product.features.length > 0"
                class="flex flex-col gap-3 mb-8"
              >
                <div
                  v-for="(feature, idx) in product.features"
                  :key="`feature-${idx}`"
                  class="flex items-center gap-2 text-sm text-muted-foreground"
                >
                  <component
                    :is="feature.icon"
                    class="w-4 h-4 flex-shrink-0 text-foreground"
                  />
                  <span>{{ feature.label }}</span>
                </div>
              </div>

              <!-- Download Button -->
              <div class="mt-auto">
                <Button
                  size="lg"
                  :disabled="isButtonDisabled(product.status)"
                  :as-child="product.status === 'available'"
                >
                  <a
                    v-if="product.status === 'available'"
                    :href="product.downloadUrl"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    {{ getButtonText(product.status) }}
                  </a>
                  <span v-else>
                    {{ getButtonText(product.status) }}
                  </span>
                </Button>
              </div>
            </div>

            <!-- Right: Screenshots Carousel (~60% on desktop) -->
            <div class="flex-1 min-w-0">
              <div v-if="product.screenshots.length > 0">
                <!--
                  FIX: Arrows were clipped because:
                  1. Card had overflow-hidden (removed above)
                  2. Arrows used large negative positioning (-left-12, -right-12)
                     which pushed them outside the card boundary

                  Solution: wrap Carousel in a container with px-10 padding,
                  then position arrows inside that padding zone using left-0 / right-0.
                  This keeps arrows fully visible without relying on overflow.
                -->
                <div class="relative px-10">
                  <Carousel
                    :opts="{
                      align: 'start',
                      loop: true,
                    }"
                    class="w-full"
                  >
                    <CarouselContent class="-ml-3">
                      <CarouselItem
                        v-for="(screenshot, idx) in product.screenshots"
                        :key="`${product.id}-screenshot-${idx}`"
                        class="pl-3 basis-4/5 sm:basis-1/2 md:basis-2/5 lg:basis-2/5"
                      >
                        <div class="flex flex-col gap-3 h-full">
                          <!-- Phone frame -->
                          <div class="bg-card rounded-2xl overflow-hidden border border-border p-2 flex items-center justify-center">
                            <img
                              :src="screenshot.src"
                              :alt="`${product.name} - ${screenshot.caption}`"
                              class="h-80 md:h-96 w-full object-contain rounded-xl"
                            />
                          </div>
                          <!-- Caption -->
                          <p class="text-xs md:text-sm text-muted-foreground text-center leading-snug px-1">
                            {{ screenshot.caption }}
                          </p>
                        </div>
                      </CarouselItem>
                    </CarouselContent>

                    <!-- Arrows sit inside the px-10 padding zone -->
                    <CarouselPrevious class="left-0" />
                    <CarouselNext class="right-0" />
                  </Carousel>
                </div>
              </div>
            </div>

          </div>
        </Card>
      </div>
    </div>
  </section>
</template>