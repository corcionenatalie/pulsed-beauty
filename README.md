import React, { useState } from "react";
import {
  BrowserRouter,
  Routes,
  Route,
  NavLink,
  useLocation,
} from "react-router-dom";
import { motion, AnimatePresence } from "framer-motion";
import {
  Sparkles,
  Calendar,
  MapPin,
  Phone,
  Mail,
  Instagram,
  Star,
  Check,
  Shield,
  Zap,
  Flower2,
  ChevronDown,
  ArrowRight,
  Heart,
  Image as ImageIcon,
  FileText,
} from "lucide-react";

/**
 * Pulsed Beauty — Multi-page React site (single-file)
 *
 * EASIEST EDITS:
 *  - Update BRAND / SERVICES / PRICING / FAQ / POLICIES / THEME below.
 *
 * Pages:
 *  / (Home)
 *  /services
 *  /pricing (optional)
 *  /about
 *  /faq
 *  /contact
 */

// ----------------------
// 1) EDIT THESE (CONTENT)
// ----------------------

const BRAND = {
  name: "Pulsed Beauty",
  tagline: "Bright results. Calm energy.",
  subtag:
    "Laser hair removal, body sculpting, teeth whitening, permanent makeup, and facials—personalized, modern, and welcoming.",

  bookingUrl: "https://your-booking-link.com", // TODO
  instagramUrl: "https://instagram.com/yourhandle", // TODO
  phone: "(000) 000-0000", // TODO
  email: "hello@pulsedbeauty.com", // TODO
  locationLine: "Your City, State", // TODO
  hoursLine: "By appointment • Evenings & weekends available", // TODO

  legalName: "Pulsed Beauty",
};

// ----------------------
// 1A) EDIT COLORS HERE (THEME)
// ----------------------
// Option A: VERY soft blush + warm “champagne” glow.
// Tip: Want a touch more pink? change rose-600 → rose-500.
const THEME = {
  primary: "bg-rose-600 text-white hover:bg-rose-700",
  blushText: "text-rose-700",
  blushBorder: "border-rose-200/70",
  glowTop: "bg-amber-100/55",
  glowLeft: "bg-rose-100/55",
  glowRight: "bg-rose-100/45",
};

const SERVICES = [
  {
    id: "laser",
    title: "Laser Hair Removal",
    icon: Zap,
    short: "Low-maintenance, silky skin.",
    description:
      "Personalized laser plans based on your goals, hair type, and comfort. Quick sessions, clear aftercare, and a schedule that fits real life.",
    bullets: [
      "Customized treatment plan",
      "Fast appointments",
      "Aftercare guidance included",
    ],
    note: "Best results come from a series. Ask about packages during your consult.",
  },
  {
    id: "sculpt",
    title: "Body Sculpting",
    icon: Shield,
    short: "Targeted contour & smoothing.",
    description:
      "We map out a plan to help define and smooth stubborn areas. Built around your lifestyle—with supportive guidance for maintenance.",
    bullets: [
      "Personalized contour mapping",
      "Great for stubborn areas",
      "Pairs well with lymphatic support",
    ],
    note: "Not a weight-loss substitute—ideal for contouring and tightening goals.",
  },
  {
    id: "whiten",
    title: "Teeth Whitening",
    icon: Sparkles,
    short: "A brighter, photo-ready smile.",
    description:
      "A confidence boost in one visit—gentle, effective, and designed with sensitivity in mind.",
    bullets: [
      "In-studio whitening session",
      "Sensitivity-conscious approach",
      "Aftercare + maintenance tips",
    ],
    note: "Avoid dark foods/drinks for 24 hours post-treatment for best results.",
  },
  {
    id: "pmu",
    title: "Permanent Makeup",
    icon: Star,
    short: "Soft, natural enhancement.",
    description:
      "Natural-looking enhancement designed to flatter your features and simplify your routine. Consult-first so we can tailor shape and tone.",
    bullets: [
      "Shape + color tailored to you",
      "Consult-first approach",
      "Aftercare guidance provided",
    ],
    note: "A patch test may be recommended. Healing time varies by service.",
  },
  {
    id: "facials",
    title: "Facials",
    icon: Flower2,
    short: "Hydration, clarity, glow.",
    description:
      "Glow-focused skincare with a results-first mindset—hydration, clarity, and barrier support in a calming, modern setting.",
    bullets: [
      "Custom facial for your skin",
      "Relaxing + effective",
      "Home-care recommendations",
    ],
    note: "Perfect monthly reset—ask about memberships or bundles.",
  },
];

const PRICING = {
  showPricing: true,
  note:
    "Prices can vary by area/service details. Book a consult and we’ll confirm the best plan for you.",
  categories: [
    {
      title: "Laser Hair Removal",
      items: [
        { name: "Small area", price: "$X" },
        { name: "Medium area", price: "$X" },
        { name: "Large area", price: "$X" },
        { name: "Packages", price: "Ask" },
      ],
    },
    {
      title: "Body Sculpting",
      items: [
        { name: "Single session", price: "$X" },
        { name: "Series (recommended)", price: "$X" },
      ],
    },
    {
      title: "Teeth Whitening",
      items: [
        { name: "Whitening session", price: "$X" },
        { name: "Touch-up", price: "$X" },
      ],
    },
    {
      title: "Permanent Makeup",
      items: [
        { name: "Consultation", price: "$X" },
        { name: "Service", price: "$X" },
        { name: "Touch-up", price: "$X" },
      ],
    },
    {
      title: "Facials",
      items: [
        { name: "Signature facial", price: "$X" },
        { name: "Advanced facial", price: "$X" },
        { name: "Membership", price: "Ask" },
      ],
    },
  ],
};

const FAQ = [
  {
    q: "Do I need a consultation?",
    a: "For most services, we recommend a quick consult so we can confirm candidacy, review goals, and customize your plan. Permanent makeup typically requires a consult.",
  },
  {
    q: "How do I prep for laser hair removal?",
    a: "Avoid waxing/threading for several weeks prior. Shave the area 12–24 hours before. Skip active skincare on the area as instructed.",
  },
  {
    q: "Is body sculpting painful?",
    a: "Comfort varies by modality and area, but most clients describe it as tolerable. We’ll explain what to expect and adjust as needed.",
  },
  {
    q: "How long do teeth whitening results last?",
    a: "It depends on lifestyle (coffee/tea/red wine/smoking). Many clients maintain results for months with good habits and occasional touch-ups.",
  },
  {
    q: "What’s the downtime for permanent makeup?",
    a: "Healing is usually a few days to a couple of weeks depending on the service. You’ll receive detailed aftercare to protect pigment and ensure the best outcome.",
  },
];

const POLICIES = {
  cancellation:
    "Please provide at least 24 hours notice to reschedule or cancel. Same-day cancellations or no-shows may be subject to a fee.",
  late:
    "If you’re running late, please text/call us. Arriving 10+ minutes late may require rescheduling to protect the next client’s time.",
  results:
    "Individual results vary and depend on consistency, home care, and individual factors. We’ll always recommend the safest, most effective plan for you.",
};

// ----------------------
// 2) UI HELPERS
// ----------------------

function cn(...classes) {
  return classes.filter(Boolean).join(" ");
}

function usePageTitle(title) {
  React.useEffect(() => {
    document.title = title ? `${title} • ${BRAND.name}` : BRAND.name;
  }, [title]);
}

function ScrollToTop() {
  const { pathname } = useLocation();
  React.useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, [pathname]);
  return null;
}

function Container({ children, className }) {
  return (
    <div className={cn("mx-auto w-full max-w-6xl px-4 md:px-6", className)}>
      {children}
    </div>
  );
}

function GlassCard({ className, children }) {
  return (
    <div
      className={cn(
        "rounded-2xl border border-neutral-200/60 bg-white/70 p-5 shadow-[0_18px_60px_-35px_rgba(0,0,0,0.35)] backdrop-blur",
        className
      )}
    >
      {children}
    </div>
  );
}

function PrimaryButton({ href, children, className, onClick }) {
  const Tag = href ? "a" : "button";
  const props = href ? { href } : { type: "button", onClick };
  return (
    <Tag
      {...props}
      className={cn(
        `inline-flex items-center justify-center gap-2 rounded-full px-5 py-2.5 text-sm font-semibold text-white shadow-sm transition ${THEME.primary}`,
        className
      )}
    >
      {children} <ArrowRight className="h-4 w-4" />
    </Tag>
  );
}

function SecondaryButton({ href, children, className, onClick }) {
  const Tag = href ? "a" : "button";
  const props = href ? { href } : { type: "button", onClick };
  return (
    <Tag
      {...props}
      className={cn(
        "inline-flex items-center justify-center gap-2 rounded-full border border-neutral-200 bg-white px-5 py-2.5 text-sm font-semibold text-neutral-900 transition hover:bg-neutral-50",
        className
      )}
    >
      {children}
    </Tag>
  );
}

function TopBadge({ children }) {
  return (
    <span
      className={cn(
        "inline-flex items-center gap-2 rounded-full border bg-white px-3 py-1 text-xs text-neutral-700",
        THEME.blushBorder
      )}
    >
      <Sparkles className={cn("h-3.5 w-3.5", THEME.blushText)} />
      {children}
    </span>
  );
}

function SectionTitle({ kicker, title, subtitle, align = "center" }) {
  return (
    <div
      className={cn("mx-auto max-w-3xl", align === "center" ? "text-center" : "")}
    >
      {kicker ? (
        <div className={cn(align === "center" ? "flex justify-center" : "")}>
          <span
            className={cn(
              "mb-3 inline-flex items-center gap-2 rounded-full border bg-white px-4 py-1 text-xs tracking-wide text-neutral-700",
              THEME.blushBorder
            )}
          >
            <Sparkles className={cn("h-3.5 w-3.5", THEME.blushText)} /> {kicker}
          </span>
        </div>
      ) : null}
      <h2 className="text-balance text-3xl font-semibold tracking-tight text-neutral-900 md:text-4xl">
        {title}
      </h2>
      {subtitle ? (
        <p className="mt-3 text-balance text-sm leading-relaxed text-neutral-600 md:text-base">
          {subtitle}
        </p>
      ) : null}
    </div>
  );
}

function PageShell({ title, children }) {
  usePageTitle(title);
  return (
    <div className="min-h-screen bg-gradient-to-b from-white via-neutral-50 to-white text-neutral-900">
      <div aria-hidden className="pointer-events-none fixed inset-0">
        <div
          className={cn(
            "absolute -top-52 left-1/2 h-[520px] w-[520px] -translate-x-1/2 rounded-full blur-[120px]",
            THEME.glowTop
          )}
        />
        <div
          className={cn(
            "absolute bottom-[-220px] left-[-160px] h-[520px] w-[520px] rounded-full blur-[130px]",
            THEME.glowLeft
          )}
        />
        <div
          className={cn(
            "absolute right-[-220px] top-[20%] h-[520px] w-[520px] rounded-full blur-[130px]",
            THEME.glowRight
          )}
        />
      </div>

      <Header />

      <div className="relative">
        <main className="pb-14">{children}</main>
        <Footer />
      </div>
    </div>
  );
}

function Header() {
  return (
    <header className="sticky top-0 z-50 border-b border-neutral-200/70 bg-white/80 backdrop-blur">
      <Container className="flex items-center justify-between gap-4 py-3">
        <a href="/" className="flex items-center gap-2">
          <div className="grid h-9 w-9 place-items-center rounded-xl border border-neutral-200 bg-white">
            <Zap className={cn("h-4 w-4", THEME.blushText)} />
          </div>
          <div className="leading-tight">
            <div className="text-sm font-semibold tracking-tight text-neutral-900">
              {BRAND.name}
            </div>
            <div className="text-[11px] text-neutral-500">Aesthetics Studio</div>
          </div>
        </a>

        <nav className="hidden items-center gap-1 md:flex">
          <NavItem to="/services">Services</NavItem>
          {PRICING.showPricing ? <NavItem to="/pricing">Pricing</NavItem> : null}
          <NavItem to="/about">About</NavItem>
          <NavItem to="/faq">FAQ</NavItem>
          <NavItem to="/contact">Contact</NavItem>
        </nav>

        <div className="flex items-center gap-2">
          <a
            href={BRAND.bookingUrl}
            className={cn(
              "hidden rounded-full border bg-white px-4 py-2 text-sm font-semibold text-neutral-900 transition hover:bg-neutral-50 md:inline-flex",
              THEME.blushBorder
            )}
          >
            Book
          </a>
          <PrimaryButton href={BRAND.bookingUrl} className="text-sm">
            Book Now
          </PrimaryButton>
        </div>
      </Container>

      <div className="border-t border-neutral-200/70 bg-white md:hidden">
        <Container className="flex items-center justify-between py-2">
          <div className="flex gap-2 overflow-x-auto">
            <MobileNav to="/services" label="Services" icon={Sparkles} />
            {PRICING.showPricing ? (
              <MobileNav to="/pricing" label="Pricing" icon={FileText} />
            ) : null}
            <MobileNav to="/about" label="About" icon={Heart} />
            <MobileNav to="/faq" label="FAQ" icon={ChevronDown} />
            <MobileNav to="/contact" label="Contact" icon={Mail} />
          </div>
        </Container>
      </div>
    </header>
  );
}

function NavItem({ to, children }) {
  return (
    <NavLink
      to={to}
      className={({ isActive }) => {
        const activeCls = THEME.primary.split(" ").slice(0, 2).join(" ");
        return cn(
          "rounded-full px-3 py-2 text-sm transition",
          isActive ? activeCls : "text-neutral-700 hover:bg-neutral-100"
        );
      }}
      end={to === "/"}
    >
      {children}
    </NavLink>
  );
}

function MobileNav({ to, label, icon: Icon }) {
  return (
    <NavLink
      to={to}
      className={({ isActive }) => {
        const activeCls = THEME.primary.split(" ").slice(0, 2).join(" ");
        return cn(
          "inline-flex shrink-0 items-center gap-2 rounded-full border border-neutral-200 bg-white px-3 py-1.5 text-xs font-semibold",
          isActive ? activeCls : "text-neutral-900"
        );
      }}
      end={to === "/"}
    >
      <Icon className={cn("h-3.5 w-3.5", THEME.blushText)} />
      {label}
    </NavLink>
  );
}

function Footer() {
  return (
    <footer className="relative border-t border-neutral-200/70 bg-white/80 py-8 backdrop-blur">
      <Container>
        <div className="flex flex-col items-center justify-between gap-3 text-center text-xs text-neutral-600 md:flex-row md:text-left">
          <div>
            © {new Date().getFullYear()} {BRAND.legalName}. All rights reserved.
          </div>
          <div className="flex items-center gap-3">
            <a className="hover:text-neutral-900" href={BRAND.bookingUrl}>
              Book
            </a>
            <span className="text-neutral-300">•</span>
            <a className="hover:text-neutral-900" href={BRAND.instagramUrl}>
              Instagram
            </a>
          </div>
        </div>
        <div className="mt-4 text-center text-[11px] text-neutral-500">
          Disclaimer: Individual results vary. Some services may have contraindications—consultation recommended.
        </div>
      </Container>
    </footer>
  );
}

function FAQItem({ q, a }) {
  const [open, setOpen] = useState(false);
  return (
    <button type="button" onClick={() => setOpen((v) => !v)} className="w-full text-left">
      <div className="flex items-start justify-between gap-4">
        <div>
          <div className="text-sm font-semibold text-neutral-900">{q}</div>
          <AnimatePresence initial={false}>
            {open ? (
              <motion.p
                initial={{ height: 0, opacity: 0 }}
                animate={{ height: "auto", opacity: 1 }}
                exit={{ height: 0, opacity: 0 }}
                transition={{ duration: 0.25 }}
                className="mt-2 overflow-hidden text-sm leading-relaxed text-neutral-600"
              >
                {a}
              </motion.p>
            ) : null}
          </AnimatePresence>
        </div>
        <div
          className={cn(
            "mt-0.5 rounded-full border border-neutral-200 p-2 text-neutral-700 transition",
            open ? "bg-neutral-100" : "bg-white"
          )}
        >
          <ChevronDown className={cn("h-4 w-4 transition", open ? "rotate-180" : "")} />
        </div>
      </div>
      <div className="mt-5 h-px w-full bg-neutral-200/70" />
    </button>
  );
}

// ----------------------
// 3) PAGES
// ----------------------

function HomePage() {
  usePageTitle("");
  return (
    <PageShell title="">
      <section className="relative">
        <Container className="grid items-center gap-10 pb-12 pt-14 md:grid-cols-2 md:pb-16 md:pt-16">
          <div>
            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.5 }}
              className="flex flex-wrap gap-2"
            >
              <TopBadge>Consult-first care</TopBadge>
              <TopBadge>Results-driven</TopBadge>
              <TopBadge>Bright & welcoming</TopBadge>
            </motion.div>

            <motion.h1
              initial={{ opacity: 0, y: 14 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.55, delay: 0.05 }}
              className="mt-6 text-balance text-4xl font-semibold tracking-tight text-neutral-900 md:text-6xl"
            >
              {BRAND.tagline}
            </motion.h1>

            <motion.p
              initial={{ opacity: 0, y: 14 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.55, delay: 0.12 }}
              className="mt-4 max-w-xl text-pretty text-sm leading-relaxed text-neutral-600 md:text-base"
            >
              {BRAND.subtag}
            </motion.p>

            <motion.div
              initial={{ opacity: 0, y: 14 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.55, delay: 0.18 }}
              className="mt-7 flex flex-col gap-3 sm:flex-row sm:items-center"
            >
              <PrimaryButton href={BRAND.bookingUrl}>Book an Appointment</PrimaryButton>
              <SecondaryButton href="/services">Explore Services</SecondaryButton>
            </motion.div>

            <div className="mt-7 flex flex-wrap gap-4 text-sm text-neutral-600">
              <div className="inline-flex items-center gap-2">
                <MapPin className={cn("h-4 w-4", THEME.blushText)} /> {BRAND.locationLine}
              </div>
              <div className="inline-flex items-center gap-2">
                <Calendar className={cn("h-4 w-4", THEME.blushText)} /> {BRAND.hoursLine}
              </div>
            </div>
          </div>

          <motion.div
            initial={{ opacity: 0, y: 16 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, delay: 0.1 }}
            className="relative"
          >
            <GlassCard className="p-0 overflow-hidden">
              <div className="relative aspect-[4/3] w-full">
                <div className="absolute inset-0 bg-[radial-gradient(circle_at_30%_20%,rgba(0,0,0,0.08),transparent_55%),radial-gradient(circle_at_70%_70%,rgba(0,0,0,0.05),transparent_60%),linear-gradient(135deg,rgba(255,255,255,0.9),rgba(255,255,255,0.55))]" />
                <div className="absolute inset-0 bg-[linear-gradient(to_top,rgba(255,255,255,0.65),transparent_55%)]" />

                <div className="absolute bottom-0 left-0 right-0 p-5">
                  <div className="flex items-center justify-between">
                    <div>
                      <div className="text-sm font-semibold text-neutral-900">Personalized Plans</div>
                      <div className="mt-1 text-xs text-neutral-600">Laser • Sculpt • Glow</div>
                    </div>
                    <div className={cn("rounded-full border bg-white px-3 py-1 text-xs text-neutral-700", THEME.blushBorder)}>
                      Consult-first
                    </div>
                  </div>

                  <div className="mt-4 grid grid-cols-3 gap-2">
                    {[
                      { label: "Laser", value: "Smooth" },
                      { label: "Sculpt", value: "Defined" },
                      { label: "Facials", value: "Glowing" },
                    ].map((m) => (
                      <div key={m.label} className="rounded-xl border border-neutral-200 bg-white/70 p-3">
                        <div className="text-[11px] text-neutral-500">{m.label}</div>
                        <div className="mt-1 text-sm font-semibold text-neutral-900">{m.value}</div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            </GlassCard>

            <div className="absolute -bottom-6 -left-6 hidden md:block">
              <GlassCard className="w-[240px]">
                <div className="text-xs text-neutral-600">Client favorites</div>
                <div className="mt-3 space-y-2">
                  {["Underarm laser", "Teeth whitening", "Glow facial"].map((t) => (
                    <div key={t} className="flex items-center gap-2 text-sm">
                      <Check className={cn("h-4 w-4", THEME.blushText)} />
                      <span className="text-neutral-900">{t}</span>
                    </div>
                  ))}
                </div>
              </GlassCard>
            </div>
          </motion.div>
        </Container>
      </section>

      <section className="relative">
        <Container className="py-10">
          <SectionTitle
            kicker="Popular services"
            title="A menu built for real results"
            subtitle="Clean, modern treatments that keep you looking like you—just elevated."
          />

          <div className="mt-10 grid gap-5 md:grid-cols-3">
            {SERVICES.slice(0, 3).map((s) => {
              const Icon = s.icon;
              return (
                <GlassCard key={s.id} className="p-6">
                  <div className="flex items-start justify-between gap-3">
                    <div className="grid h-10 w-10 place-items-center rounded-xl border border-neutral-200 bg-white">
                      <Icon className={cn("h-4 w-4", THEME.blushText)} />
                    </div>
                    <span className={cn("rounded-full border bg-white px-3 py-1 text-xs text-neutral-700", THEME.blushBorder)}>
                      Most booked
                    </span>
                  </div>
                  <div className="mt-4 text-lg font-semibold text-neutral-900">{s.title}</div>
                  <p className="mt-2 text-sm leading-relaxed text-neutral-600">{s.short}</p>
                  <div className="mt-4">
                    <SecondaryButton href={`/services#${s.id}`}>Learn more</SecondaryButton>
                  </div>
                </GlassCard>
              );
            })}
          </div>

          <div className="mt-8 flex flex-col items-center justify-center gap-3 sm:flex-row">
            <PrimaryButton href={BRAND.bookingUrl}>Book Now</PrimaryButton>
            <SecondaryButton href="/services">View all services</SecondaryButton>
          </div>
        </Container>
      </section>
    </PageShell>
  );
}

function ServicesPage() {
  usePageTitle("Services");
  const [selected, setSelected] = useState(SERVICES[0]);

  return (
    <PageShell title="Services">
      <Container className="py-12">
        <SectionTitle
          kicker="Services"
          title="Treatments that fit your life"
          subtitle="Pick a service to see details, add-ons, and what to expect."
        />

        <div className="mt-10 grid gap-6 md:grid-cols-12">
          <div className="md:col-span-5">
            <div className="grid gap-3">
              {SERVICES.map((s) => {
                const Icon = s.icon;
                const isActive = selected.id === s.id;
                return (
                  <button
                    key={s.id}
                    id={s.id}
                    type="button"
                    onClick={() => setSelected(s)}
                    className={cn(
                      "group rounded-2xl border p-4 text-left transition",
                      isActive ? "border-neutral-300 bg-white" : "border-neutral-200 bg-white/70 hover:bg-white"
                    )}
                  >
                    <div className="flex items-start justify-between gap-4">
                      <div className="flex items-start gap-3">
                        <div
                          className={cn(
                            "grid h-10 w-10 place-items-center rounded-xl border",
                            isActive ? "border-neutral-300 bg-neutral-50" : "border-neutral-200 bg-white"
                          )}
                        >
                          <Icon className={cn("h-4 w-4", THEME.blushText)} />
                        </div>
                        <div>
                          <div className="text-sm font-semibold text-neutral-900">{s.title}</div>
                          <div className="mt-1 text-xs text-neutral-500">{s.short}</div>
                        </div>
                      </div>
                      <div className="mt-1 text-neutral-400 transition group-hover:text-neutral-800">
                        <ArrowRight className="h-4 w-4" />
                      </div>
                    </div>
                  </button>
                );
              })}
            </div>
          </div>

          <div className="md:col-span-7">
            <AnimatePresence mode="wait">
              <motion.div
                key={selected.id}
                initial={{ opacity: 0, y: 10 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: -10 }}
                transition={{ duration: 0.25 }}
              >
                <GlassCard className="p-6">
                  <div className="flex items-start justify-between gap-4">
                    <div>
                      <div className="text-xs text-neutral-500">Selected</div>
                      <div className="mt-1 text-2xl font-semibold tracking-tight text-neutral-900">{selected.title}</div>
                    </div>
                    <div className={cn("hidden sm:flex items-center gap-2 rounded-full border bg-white px-3 py-1 text-xs text-neutral-700", THEME.blushBorder)}>
                      <Sparkles className={cn("h-3.5 w-3.5", THEME.blushText)} /> Results-focused
                    </div>
                  </div>

                  <p className="mt-4 text-sm leading-relaxed text-neutral-600">{selected.description}</p>

                  <div className="mt-5 grid gap-3 sm:grid-cols-2">
                    {selected.bullets.map((b) => (
                      <div key={b} className="flex items-start gap-2 rounded-xl border border-neutral-200 bg-white p-3">
                        <Check className={cn("mt-0.5 h-4 w-4", THEME.blushText)} />
                        <div className="text-sm text-neutral-700">{b}</div>
                      </div>
                    ))}
                  </div>

                  <div className={cn("mt-5 rounded-2xl border bg-white p-4", THEME.blushBorder)}>
                    <div className="text-xs font-semibold text-neutral-900">Quick note</div>
                    <div className="mt-1 text-sm text-neutral-600">{selected.note}</div>
                  </div>

                  <div className="mt-6 flex flex-col gap-3 sm:flex-row sm:items-center">
                    <PrimaryButton href={BRAND.bookingUrl}>Book {selected.title}</PrimaryButton>
                    <SecondaryButton href="/contact">Ask a question</SecondaryButton>
                  </div>
                </GlassCard>
              </motion.div>
            </AnimatePresence>
          </div>
        </div>
      </Container>
    </PageShell>
  );
}

function PricingPage() {
  usePageTitle("Pricing");
  return (
    <PageShell title="Pricing">
      <Container className="py-12">
        <SectionTitle kicker="Pricing" title="Clear, simple pricing" subtitle={PRICING.note} />

        <div className="mt-10 grid gap-5 md:grid-cols-2">
          {PRICING.categories.map((cat) => (
            <GlassCard key={cat.title} className="p-6">
              <div className="text-lg font-semibold text-neutral-900">{cat.title}</div>
              <div className="mt-4 space-y-3">
                {cat.items.map((it) => (
                  <div key={it.name} className="flex items-center justify-between gap-3 rounded-xl border border-neutral-200 bg-white px-4 py-3">
                    <div className="text-sm font-semibold text-neutral-900">{it.name}</div>
                    <div className="text-sm text-neutral-600">{it.price}</div>
                  </div>
                ))}
              </div>
              <div className="mt-5 flex flex-col gap-3 sm:flex-row">
                <PrimaryButton href={BRAND.bookingUrl}>Book</PrimaryButton>
                <SecondaryButton href="/services">View services</SecondaryButton>
              </div>
            </GlassCard>
          ))}
        </div>
      </Container>
    </PageShell>
  );
}

function AboutPage() {
  usePageTitle("About");
  return (
    <PageShell title="About">
      <Container className="py-12">
        <SectionTitle
          kicker="About"
          title="Modern aesthetics, done thoughtfully"
          subtitle="Pulsed Beauty is built on a simple idea: you deserve treatments that feel luxe, make sense, and deliver."
        />

        <div className="mt-10 grid gap-6 md:grid-cols-12">
          <GlassCard className="md:col-span-7 p-6">
            <div className="text-sm font-semibold text-neutral-900">Results you can feel confident about</div>
            <p className="mt-2 text-sm leading-relaxed text-neutral-600">
              We focus on high-impact services—laser hair removal, body sculpting, teeth whitening, permanent makeup, and facials—so your routine stays
              simple and your results stay consistent.
            </p>
            <div className="mt-5 grid gap-3 sm:grid-cols-2">
              {[
                { title: "Consult-first", desc: "We customize every plan and set realistic timelines." },
                { title: "Comfort-forward", desc: "Clear steps, guidance, and aftercare you can actually follow." },
                { title: "Natural finish", desc: "Enhancement that still looks like you—just elevated." },
                { title: "Bright space", desc: "Clean, airy, and designed to feel welcoming." },
              ].map((p) => (
                <div key={p.title} className={cn("rounded-2xl border bg-white p-4", THEME.blushBorder)}>
                  <div className="text-sm font-semibold text-neutral-900">{p.title}</div>
                  <div className="mt-1 text-sm text-neutral-600">{p.desc}</div>
                </div>
              ))}
            </div>
          </GlassCard>

          <GlassCard className="md:col-span-5 p-6">
            <div className="text-sm font-semibold text-neutral-900">Add your story</div>
            <p className="mt-2 text-sm leading-relaxed text-neutral-600">
              Drop in your bio here (credentials, training, specialties), plus a short mission statement.
            </p>
            <div className={cn("mt-4 rounded-2xl border bg-white p-4", THEME.blushBorder)}>
              <div className="text-xs font-semibold text-neutral-900">Prompt</div>
              <div className="mt-1 text-sm text-neutral-600">“I started Pulsed Beauty because…”</div>
            </div>
            <div className="mt-5 flex flex-col gap-3">
              <SecondaryButton href="/contact">Send your info</SecondaryButton>
              <a
                href={BRAND.instagramUrl}
                className={cn(
                  "inline-flex items-center justify-center gap-2 rounded-full border bg-white px-5 py-2.5 text-sm font-semibold text-neutral-900 transition hover:bg-neutral-50",
                  THEME.blushBorder
                )}
              >
                <Instagram className={cn("h-4 w-4", THEME.blushText)} /> Instagram
              </a>
            </div>
          </GlassCard>
        </div>
      </Container>
    </PageShell>
  );
}

function FAQPage() {
  usePageTitle("FAQ");
  return (
    <PageShell title="FAQ">
      <Container className="py-12">
        <SectionTitle
          kicker="FAQ"
          title="Questions, answered"
          subtitle="If you don’t see your question here, message us and we’ll help you choose the right service."
        />

        <div className="mt-10 grid gap-6 md:grid-cols-12">
          <GlassCard className="md:col-span-7 p-6">
            <div className="space-y-1">
              {FAQ.map((f, idx) => (
                <FAQItem key={idx} q={f.q} a={f.a} />
              ))}
            </div>
          </GlassCard>

          <GlassCard className="md:col-span-5 p-6">
            <div className="text-sm font-semibold text-neutral-900">Policies</div>
            <p className="mt-2 text-sm leading-relaxed text-neutral-600">Clear expectations keep things smooth for everyone.</p>
            <div className="mt-4 space-y-3">
              {[
                { t: "Cancellation", d: POLICIES.cancellation },
                { t: "Late policy", d: POLICIES.late },
                { t: "Results", d: POLICIES.results },
              ].map((p) => (
                <div key={p.t} className={cn("rounded-2xl border bg-white p-4", THEME.blushBorder)}>
                  <div className="text-xs font-semibold text-neutral-900">{p.t}</div>
                  <div className="mt-1 text-sm text-neutral-600">{p.d}</div>
                </div>
              ))}
            </div>
            <div className="mt-5 flex flex-col gap-3">
              <PrimaryButton href={BRAND.bookingUrl}>Book a consult</PrimaryButton>
              <SecondaryButton href="/contact">Contact us</SecondaryButton>
            </div>
          </GlassCard>
        </div>
      </Container>
    </PageShell>
  );
}

function ContactPage() {
  usePageTitle("Contact");
  return (
    <PageShell title="Contact">
      <Container className="py-12">
        <SectionTitle
          kicker="Contact"
          title="Let’s get you booked"
          subtitle="Questions before you commit? Send a message and we’ll help you choose the right service."
        />

        <div className="mt-10 grid gap-6 md:grid-cols-12">
          <GlassCard className="md:col-span-6 p-6">
            <div className="text-sm font-semibold text-neutral-900">Contact info</div>
            <div className="mt-4 space-y-3">
              <InfoRow icon={Phone} title={BRAND.phone} subtitle="Call or text" />
              <InfoRow icon={Mail} title={BRAND.email} subtitle="Email us" />
              <InfoRow icon={MapPin} title={BRAND.locationLine} subtitle="Location" />
            </div>

            <div className="mt-6 flex flex-col gap-3 sm:flex-row">
              <PrimaryButton href={BRAND.bookingUrl}>Book Now</PrimaryButton>
              <a
                href={BRAND.instagramUrl}
                className={cn(
                  "inline-flex items-center justify-center gap-2 rounded-full border bg-white px-5 py-2.5 text-sm font-semibold text-neutral-900 transition hover:bg-neutral-50",
                  THEME.blushBorder
                )}
              >
                <Instagram className={cn("h-4 w-4", THEME.blushText)} /> Instagram
              </a>
            </div>

            <div className={cn("mt-6 rounded-2xl border bg-white p-4", THEME.blushBorder)}>
              <div className="text-xs font-semibold text-neutral-900">Hours</div>
              <div className="mt-1 text-sm text-neutral-600">{BRAND.hoursLine}</div>
            </div>
          </GlassCard>

          <GlassCard className="md:col-span-6 p-6">
            <div className="text-sm font-semibold text-neutral-900">Message us</div>
            <p className="mt-2 text-sm leading-relaxed text-neutral-600">
              This form is a ready-to-wire UI. To make it actually send emails, connect it to Formspree, Netlify Forms, or your backend.
            </p>

            <form
              onSubmit={(e) => {
                e.preventDefault();
                alert(
                  "Form submitted (demo). To make this send, connect it to Formspree/Netlify Forms or a backend."
                );
              }}
              className="mt-5 space-y-3"
            >
              <div className="grid gap-3 sm:grid-cols-2">
                <input
                  className={cn(
                    "h-11 w-full rounded-xl border bg-white px-4 text-sm text-neutral-900 placeholder:text-neutral-400 outline-none focus:border-neutral-400",
                    THEME.blushBorder
                  )}
                  placeholder="Name"
                  required
                />
                <input
                  className={cn(
                    "h-11 w-full rounded-xl border bg-white px-4 text-sm text-neutral-900 placeholder:text-neutral-400 outline-none focus:border-neutral-400",
                    THEME.blushBorder
                  )}
                  placeholder="Phone or email"
                  required
                />
              </div>
              <select
                className={cn(
                  "h-11 w-full rounded-xl border bg-white px-4 text-sm text-neutral-800 outline-none focus:border-neutral-400",
                  THEME.blushBorder
                )}
              >
                {[
                  "What are you interested in?",
                  "Laser hair removal",
                  "Body sculpting",
                  "Teeth whitening",
                  "Permanent makeup",
                  "Facials",
                  "Not sure—help me choose",
                ].map((o) => (
                  <option key={o} value={o} className="bg-white">
                    {o}
                  </option>
                ))}
              </select>
              <textarea
                className={cn(
                  "min-h-[120px] w-full rounded-xl border bg-white px-4 py-3 text-sm text-neutral-900 placeholder:text-neutral-400 outline-none focus:border-neutral-400",
                  THEME.blushBorder
                )}
                placeholder="Tell us your goal + any questions"
              />
              <div className="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
                <PrimaryButton className="w-full sm:w-auto">Send message</PrimaryButton>
                <div className="text-xs text-neutral-500">By sending, you agree to be contacted about your inquiry.</div>
              </div>
            </form>
          </GlassCard>
        </div>
      </Container>
    </PageShell>
  );
}

function InfoRow({ icon: Icon, title, subtitle }) {
  return (
    <div className="flex items-start gap-3">
      <div className={cn("grid h-10 w-10 place-items-center rounded-xl border bg-white", THEME.blushBorder)}>
        <Icon className={cn("h-4 w-4", THEME.blushText)} />
      </div>
      <div>
        <div className="text-sm text-neutral-900">{title}</div>
        <div className="text-xs text-neutral-500">{subtitle}</div>
      </div>
    </div>
  );
}

function NotFoundPage() {
  usePageTitle("Not found");
  return (
    <PageShell title="Not found">
      <Container className="py-16">
        <GlassCard className="p-8 text-center">
          <div className={cn("mx-auto grid h-12 w-12 place-items-center rounded-2xl border bg-white", THEME.blushBorder)}>
            <ImageIcon className={cn("h-5 w-5", THEME.blushText)} />
          </div>
          <div className="mt-4 text-2xl font-semibold text-neutral-900">Page not found</div>
          <p className="mt-2 text-sm text-neutral-600">Try heading back home.</p>
          <div className="mt-6 flex items-center justify-center gap-3">
            <PrimaryButton href="/">Home</PrimaryButton>
            <SecondaryButton href="/services">Services</SecondaryButton>
          </div>
        </GlassCard>
      </Container>
    </PageShell>
  );
}

// ----------------------
// 4) APP (Router)
// ----------------------

export default function PulsedBeautyApp() {
  return (
    <BrowserRouter>
      <ScrollToTop />
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/services" element={<ServicesPage />} />
        {PRICING.showPricing ? <Route path="/pricing" element={<PricingPage />} /> : null}
        <Route path="/about" element={<AboutPage />} />
        <Route path="/faq" element={<FAQPage />} />
        <Route path="/contact" element={<ContactPage />} />
        <Route path="*" element={<NotFoundPage />} />
      </Routes>
    </BrowserRouter>
  );
}
